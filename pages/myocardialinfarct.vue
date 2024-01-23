<template>
  <main class="text-gray-800 mt-8 pb-20 flex-grow">
    <n-page-header @back="handleBack">
      <template #title>
        <h1>
          At-Risk Myocardial Mass Calculator (<a
            href="https://doi.org/10.4244/eijv8i12a220"
            target="_blank"
            rel="noopener"
            class="text-blue-400 hover:text-blue-600 transition-all hover:underline"
            >Kassab et al, 2013</a
          >)
        </h1>
      </template>
    </n-page-header>

    <p class="text-base md:text-lg font-normal mt-3 text-slate-600"></p>

    <n-divider />

    <n-space vertical>
      <h2 class="font-medium">
        Do you want to calculate the percent infarct mass relative to the artery
        or relative to the entire heart?
      </h2>

      <n-radio-group
        v-model:value="mode"
        name="modeSelector"
        size="large"
        :on-update:value="resetModeCalculation"
      >
        <n-radio-button value="infarctArtery">
          Relative to artery
        </n-radio-button>
        <n-radio-button value="infarctHeart">
          Relative to entire heart
        </n-radio-button>
      </n-radio-group>
    </n-space>

    <n-divider />
    <n-space vertical>
      <h2 class="font-medium">
        Do you want to use artery diameters or areas for the calculation?
      </h2>

      <n-radio-group
        v-model:value="relativeUnit"
        name="measurementSelector"
        size="large"
        :on-update:value="resetRelativeUnitCalculation"
      >
        <n-radio-button value="diameter"> Diameter </n-radio-button>
        <n-radio-button value="area"> Area </n-radio-button>
      </n-radio-group>
    </n-space>

    <n-divider />

    <n-space vertical>
      <h2 class="font-medium">
        Provide the units used for the {{ relativeUnitLabel }}s:
      </h2>

      <n-radio-group v-model:value="unit" name="unitSelector" size="large">
        <n-radio-button value="squaremm">
          mm<sup v-if="relativeArea">2</sup>
        </n-radio-button>
        <n-radio-button value="squarecm">
          cm<sup v-if="relativeArea">2</sup>
        </n-radio-button>
        <n-radio-button value="squarein">
          in<sup v-if="relativeArea">2</sup>
        </n-radio-button>
      </n-radio-group>
    </n-space>

    <n-divider />

    <div class="flex flex-col mb-5">
      <h2 class="mb-3 font-medium" v-if="mode === 'infarctArtery'">
        Enter the lumen {{ relativeUnitLabel }} of the main artery and the side
        branch:
      </h2>
      <h2 class="mb-3 font-medium" v-else>
        Enter the lumen {{ relativeUnitLabel }} of the side branch, left
        coronary artery, and right coronary artery:
      </h2>

      <p
        class="text-base text-slate-600 font-normal"
        v-if="mode === 'infarctArtery'"
      >
        {{ relativeArea ? "A" : "D" }}<sub>MA</sub> - Lumen
        {{ relativeUnitLabel }} of the main artery
      </p>
      <p class="text-base text-slate-600 font-normal">
        {{ relativeArea ? "A" : "D" }}<sub>SB</sub> - Lumen
        {{ relativeUnitLabel }} of the side branch
      </p>
      <p
        class="text-base text-slate-600 font-normal"
        v-if="mode === 'infarctHeart'"
      >
        {{ relativeArea ? "A" : "D" }}<sub>LCCA</sub> - Lumen
        {{ relativeUnitLabel }} of the left coronary artery
      </p>
      <p
        class="text-base text-slate-600 font-normal"
        v-if="mode === 'infarctHeart'"
      >
        {{ relativeArea ? "A" : "D" }}<sub>RCA</sub> - Lumen
        {{ relativeUnitLabel }} of the right coronary artery
      </p>
    </div>

    <div
      class="flex lg:flex-row flex-col-reverse lg:justify-start lg:items-start"
    >
      <div class="flex flex-col">
        <div class="flex flex-row items-center space-x-4 my-4">
          <p class="text-xl font-medium w-[120px]">
            {{ relativeArea ? "A" : "D" }}<sub>SB</sub>
          </p>
          <n-input-number
            v-model:value="vSB"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'"
              >mm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarecm'"
              >cm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarein'"
              >in<sup v-if="relativeArea">2</sup></span
            >
          </p>
        </div>

        <div
          class="flex flex-row items-center space-x-4 my-4"
          v-if="mode === 'infarctArtery'"
        >
          <p class="text-xl font-medium w-[120px]">
            {{ relativeArea ? "A" : "D" }}<sub>MA</sub>
          </p>
          <n-input-number
            v-model:value="vMA"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'"
              >mm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarecm'"
              >cm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarein'"
              >in<sup v-if="relativeArea">2</sup></span
            >
          </p>
        </div>

        <div
          class="flex flex-row items-center space-x-4 my-4"
          v-if="mode === 'infarctHeart'"
        >
          <p class="text-xl font-medium w-[120px]">
            {{ relativeArea ? "A" : "D" }}<sub>LCCA</sub>
          </p>
          <n-input-number
            v-model:value="vLCCA"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'"
              >mm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarecm'"
              >cm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarein'"
              >in<sup v-if="relativeArea">2</sup></span
            >
          </p>
        </div>

        <div
          class="flex flex-row items-center space-x-4 my-4"
          v-if="mode === 'infarctHeart'"
        >
          <p class="text-xl font-medium w-[120px]">
            {{ relativeArea ? "A" : "D" }}<sub>RCA</sub>
          </p>
          <n-input-number
            v-model:value="vRCA"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'"
              >mm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarecm'"
              >cm<sup v-if="relativeArea">2</sup></span
            >
            <span v-if="unit === 'squarein'"
              >in<sup v-if="relativeArea">2</sup></span
            >
          </p>
        </div>

        <div class="w-full flex my-6 justify-center">
          <n-button
            class=""
            size="large"
            type="primary"
            :class="{
              'animation-pulse': !disableButton && !showOutput,
            }"
            @click="calculate"
            :disabled="disableButton"
          >
            Calculate
          </n-button>
        </div>
      </div>

      <div class="lg:ml-[200px] flex py-3">
        <n-image width="200" src="/images/sketch.png" />
      </div>
    </div>

    <n-collapse-transition :show="showOutput">
      <p class="text-2xl font-bold my-4">
        The percentage of myocardial mass at risk of infarct relative to the
        {{ mode === "infarctArtery" ? "artery" : "heart" }} is {{ output.val }}%
      </p>

      <n-divider />
    </n-collapse-transition>

    <n-collapse-transition :show="showOutput">
      <p class="mb-4 text-lg font-semibold text-center md:text-left">
        How is it calculated?
      </p>

      <div
        class="w-full p-4 md:p-8 flex flex-col items-center bg-amber-50 rounded-lg text-xl md:text-2xl font-medium"
      >
        <p class="reference" v-if="mode == 'infarctArtery'">
          The percentage of myocardial mass relative to the artery is calculated
          based on the relation established by
          <a
            href="https://doi.org/10.4244/eijv8i12a220"
            target="_blank"
            rel="noopener"
            class="text-blue-400 hover:text-blue-600 transition-all hover:underline"
          >
            Kassab et al (2013)
          </a>
        </p>
        <p class="reference" v-if="mode == 'infarctHeart'">
          The percentage of infarcted myocardial mass relative to the entire
          heart is calculated based on the relation established by
          <a
            href="https://doi.org/10.4244/eijv8i12a220"
            target="_blank"
            rel="noopener"
            class="text-blue-400 hover:text-blue-600 transition-all hover:underline"
          >
            Kassab et al (2013)
          </a>
        </p>

        <div
          class="flex flex-col space-y-8 items-center"
          v-if="mode == 'infarctArtery'"
        >
          <span v-html="infarctArteryEq1"></span>
          <span v-html="infarctArteryEq2" v-if="!relativeArea"></span>
          <span v-html="infarctArteryEq3" v-if="!relativeArea"></span>
          <span v-html="infarctArteryEq4"></span>

          <div class="h-[2px] w-full my-4 bg-black"></div>

          <p class="text-2xl md:text-3xl font-semibold">
            %Infarct<sub>artery</sub> = {{ output.val }}%
          </p>
        </div>

        <div
          class="flex flex-col space-y-8 items-center"
          v-if="mode == 'infarctHeart'"
        >
          <span v-html="infarctHeartEq1"></span>
          <span v-html="infarctHeartEq2"></span>
          <span v-html="infarctHeartEq3"></span>
          <span v-html="infarctHeartEq4"></span>

          <p class="text-2xl md:text-3xl font-semibold">
            %Infarct<sub>heart</sub> = {{ output.val }}%
          </p>
        </div>
      </div>
    </n-collapse-transition>
  </main>
</template>

<script setup lang="ts">
import katex from "katex";
import "katex/dist/katex.css";

const title = "At-Risk Myocardial Mass Calculator";
const description = "";

useHead({
  title,
  meta: [
    {
      name: "description",
      content: description,
    },
    {
      name: "og:image",
      content: `https://kalai.fairdataihub.org/api/generate?app=cardiac-calculators.com&title=${encodeURI(
        title
      )}&org=fairdataihub&description=${description}`,
    },
  ],
});

const infarctArteryEq1 = ref("");
const infarctArteryEq2 = ref("");
const infarctArteryEq3 = ref("");
const infarctArteryEq4 = ref("");

const infarctHeartEq1 = ref("");
const infarctHeartEq2 = ref("");
const infarctHeartEq3 = ref("");
const infarctHeartEq4 = ref("");

onMounted(() => {
  infarctArteryEq1.value = katex.renderToString(
    "\\% Infarct_{heart} = \\left(\\frac{A_{SB}^{\\frac{4}{3}}}{A_{LCCA}^{\\frac{4}{3}} + A_{RCA}^{\\frac{4}{3}}}\\right) \\times 100",
    {
      throwOnError: true,
    }
  );
});

const mode = ref("infarctArtery");
const relativeUnit = ref("diameter");
const unit = ref("squaremm");

const vMA = ref();
const vSB = ref();
const vLCCA = ref();
const vRCA = ref();

const showOutput = ref(false);

const output = ref({
  label: "",
  val: "",
});

const relativeUnitLabel = computed(() => {
  if (relativeUnit.value === "area") {
    return "area";
  } else if (relativeUnit.value === "diameter") {
    return "diameter";
  }
});

const relativeArea = computed(() => {
  if (relativeUnit.value === "area") {
    return true;
  } else if (relativeUnit.value === "diameter") {
    return false;
  }
});

const handleBack = async () => {
  await navigateTo("/");
};

const toFixedIfNecessary = (value: number, dp: number) => {
  return +parseFloat(value.toFixed(dp));
};

const resetModeCalculation = (value: string) => {
  showOutput.value = false;

  if (value === "infarctArtery") {
    vMA.value = null;
    vSB.value = null;
  } else if (value === "infarctHeart") {
    vSB.value = null;
    vLCCA.value = null;
    vRCA.value = null;
  }

  mode.value = value;
};

const resetRelativeUnitCalculation = (value: string) => {
  showOutput.value = false;

  if (value === "area") {
    vMA.value = null;
    vSB.value = null;
    vLCCA.value = null;
    vRCA.value = null;
  } else if (value === "diameter") {
    vMA.value = null;
    vSB.value = null;
    vLCCA.value = null;
    vRCA.value = null;
  }

  relativeUnit.value = value;
};

const hideOutput = () => {
  showOutput.value = false;
};

const placeholder = computed(() => {
  if (disableButton.value) {
    return "";
  } else {
    return "Enter area";
  }
});

const disableButton = computed(() => {
  if (mode.value === "infarctArtery") {
    if (emptyInput(vMA.value) || emptyInput(vSB.value)) {
      return true;
    }
  } else if (mode.value === "infarctHeart") {
    if (
      emptyInput(vSB.value) ||
      emptyInput(vLCCA.value) ||
      emptyInput(vRCA.value)
    ) {
      return true;
    }
  }

  return false;
});

const emptyInput = (val: any) => {
  if (val === undefined || val === null) {
    return true;
  }
  return false;
};

declare global {
  interface Window {
    umami: {
      track: (eventName: string, eventData: Record<string, unknown>) => void;
    };
  }
}

const calculate = () => {
  window.umami.track(mode.value, {
    vMA: vMA.value,
    vSB: vSB.value,
    vLCCA: vLCCA.value,
    vRCA: vRCA.value,
    unit: relativeUnit.value,
  });

  if (mode.value === "infarctArtery") {
    const a1 = relativeArea.value
      ? vSB.value
      : Math.PI * Math.pow(vSB.value / 2, 2);

    const a2 = relativeArea.value
      ? vMA.value
      : Math.PI * Math.pow(vMA.value / 2, 2);

    const n1 = a1 / a2;
    const n2 = Math.pow(n1, 4 / 3);
    const n3 = n2 * 100;

    output.value.val = n3.toFixed(2);
    output.value.label = "%";

    showOutput.value = true;

    infarctArteryEq1.value = katex.renderToString(
      "\\% Infarct_{artery} = \\left(\\frac{A_{SB}}{A_{MA}}\\right)^{\\frac{4}{3}} \\times 100",

      {
        throwOnError: false,
      }
    );

    if (relativeUnit.value === "diameter") {
      infarctArteryEq2.value = katex.renderToString(
        `\\% Infarct_{artery} = \\left( \\frac{{\\pi \\times {\\left(\\frac{D_{SB}}{2} \\right)^{2}}}}{ \\pi \\times \\left( \\frac{D_{MA}}{2} \\right) ^ 2 } \\right) ^ \\frac{4}{3} \\times 100`,
        {
          throwOnError: false,
        }
      );

      infarctArteryEq3.value = katex.renderToString(
        "\\% Infarct_{artery} = \\left(\\frac{\\pi \\times \\left(\\frac{" +
          vSB.value +
          "}{2} \\right)^{2}}{\\pi \\times \\left(\\frac{" +
          vMA.value +
          "}{2} \\right)^{2}}\\right)^{\\frac{4}{3}} \\times 100",
        {
          throwOnError: false,
        }
      );
    }

    infarctArteryEq4.value = katex.renderToString(
      "\\% Infarct_{artery} = \\left(\\frac{" +
        toFixedIfNecessary(a1, 4) +
        "}{" +
        toFixedIfNecessary(a2, 4) +
        "}\\right)^{\\frac{4}{3}} \\times 100",
      {
        throwOnError: false,
      }
    );
  } else if (mode.value === "infarctHeart") {
    const a1 = relativeArea.value
      ? vSB.value
      : Math.PI * Math.pow(vSB.value / 2, 2);

    const a2 = relativeArea.value
      ? vLCCA.value
      : Math.PI * Math.pow(vLCCA.value / 2, 2);

    const a3 = relativeArea.value
      ? vRCA.value
      : Math.PI * Math.pow(vRCA.value / 2, 2);

    const n1 = Math.pow(a1, 4 / 3);
    const n2 = Math.pow(a2, 4 / 3);
    const n3 = Math.pow(a3, 4 / 3);

    const n4 = n1 / (n2 + n3);
    const n5 = n4 * 100;

    output.value.val = n5.toFixed(2);
    output.value.label = "%";

    showOutput.value = true;

    infarctHeartEq1.value = katex.renderToString(
      "\\% Infarct_{heart} = \\left(\\frac{A_{SB}^{\\frac{4}{3}}}{A_{LCCA}^{\\frac{4}{3}} + A_{RCA}^{\\frac{4}{3}}}\\right) \\times 100",
      {
        throwOnError: false,
      }
    );

    if (relativeUnit.value === "diameter") {
      infarctHeartEq2.value = katex.renderToString(
        `\\% Infarct_{heart} = \\left( \\frac{{\\pi \\times {\\left(\\frac{D_{SB}}{2} \\right)^{2}}}}{ \\pi \\times \\left( \\frac{D_{LCCA}}{2} \\right) ^ 2 + \\pi \\times \\left( \\frac{D_{RCA}}{2} \\right) ^ 2 } \\right) ^ \\frac{4}{3} \\times 100`,
        {
          throwOnError: false,
        }
      );

      infarctHeartEq3.value = katex.renderToString(
        "\\% Infarct_{heart} = \\left(\\frac{\\pi \\times \\left(\\frac{" +
          vSB.value +
          "}{2} \\right)^{2}}{\\pi \\times \\left(\\frac{" +
          vLCCA.value +
          "}{2} \\right)^{2} + \\pi \\times \\left(\\frac{" +
          vRCA.value +
          "}{2} \\right)^{2}}\\right)^{\\frac{4}{3}} \\times 100",
        {
          throwOnError: false,
        }
      );
    }

    infarctHeartEq4.value = katex.renderToString(
      "\\% Infarct_{heart} = \\left(\\frac{" +
        toFixedIfNecessary(a1, 4) +
        "^{\\frac{4}{3}}}{" +
        toFixedIfNecessary(a2, 4) +
        "^{\\frac{4}{3}} + " +
        toFixedIfNecessary(a3, 4) +
        "^{\\frac{4}{3}}}\\right) \\times 100",
      {
        throwOnError: false,
      }
    );
  }
};
</script>

<style scoped>
.reference {
  @apply text-lg text-slate-600 font-medium mb-4 text-center md:text-left;
}
</style>
