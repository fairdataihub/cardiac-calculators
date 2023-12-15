<template>
  <main class="text-gray-800 mt-8 pb-20 flex-grow">
    <h1>
      Calculator for the relation of side branch diameter to perfused myocardial
      mass
    </h1>

    <p class="text-base md:text-lg font-normal mt-3 text-slate-600">
      Explanatory text for the calculator
    </p>

    <n-divider />

    <h2 class="mb-4 font-medium">
      Is the Infarct percentage relative to the entire heart?
    </h2>

    <n-radio-group
      v-model:value="mode"
      name="modeSelector"
      size="large"
      :on-update:value="resetCalculation"
    >
      <n-radio-button value="infarctArtery"> No </n-radio-button>
      <n-radio-button value="infarctHeart"> Yes </n-radio-button>
    </n-radio-group>

    <n-divider />

    <div class="flex flex-col justify-start items-start">
      <h2 class="font-medium">Provide the units used for the areas:</h2>

      <n-radio-group v-model:value="unit" name="unitSelector" size="large">
        <n-radio-button value="squaremm"> mm<sup>2</sup> </n-radio-button>
        <n-radio-button value="squarecm"> cm<sup>2</sup> </n-radio-button>
        <n-radio-button value="squarein"> in<sup>2</sup> </n-radio-button>
      </n-radio-group>
    </div>

    <n-divider />

    <div class="flex flex-col mb-5">
      <h2 class="mb-3 font-medium" v-if="mode === 'infarctArtery'">
        Enter the lumen area of the main artery and the side branch:
      </h2>
      <h2 class="mb-3 font-medium" v-else>
        Enter the lumen area of the side branch, left coronary artery, and right
        coronary artery:
      </h2>

      <p
        class="text-base text-slate-600 font-normal"
        v-if="mode === 'infarctArtery'"
      >
        A<sub>MA</sub> - Lumen area of the main artery
      </p>
      <p class="text-base text-slate-600 font-normal">
        A<sub>SB</sub> - Lumen area of the side branch
      </p>
      <p
        class="text-base text-slate-600 font-normal"
        v-if="mode === 'infarctHeart'"
      >
        A<sub>LCCA</sub> - Lumen area of the left coronary artery
      </p>
      <p
        class="text-base text-slate-600 font-normal"
        v-if="mode === 'infarctHeart'"
      >
        A<sub>RCA</sub> - Lumen area of the right coronary artery
      </p>
    </div>

    <div
      class="flex lg:flex-row flex-col-reverse lg:justify-start lg:items-start"
    >
      <div class="flex flex-col">
        <div class="flex flex-row items-center space-x-4 my-4">
          <p class="text-xl font-medium w-[120px]">A<sub>SB</sub></p>
          <n-input-number
            v-model:value="aSB"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'">mm<sup>2</sup></span>
            <span v-if="unit === 'squarecm'">cm<sup>2</sup></span>
            <span v-if="unit === 'squarein'">in<sup>2</sup></span>
          </p>
        </div>

        <div
          class="flex flex-row items-center space-x-4 my-4"
          v-if="mode === 'infarctArtery'"
        >
          <p class="text-xl font-medium w-[120px]">A<sub>MA</sub></p>
          <n-input-number
            v-model:value="aMA"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'">mm<sup>2</sup></span>
            <span v-if="unit === 'squarecm'">cm<sup>2</sup></span>
            <span v-if="unit === 'squarein'">in<sup>2</sup></span>
          </p>
        </div>

        <div
          class="flex flex-row items-center space-x-4 my-4"
          v-if="mode === 'infarctHeart'"
        >
          <p class="text-xl font-medium w-[120px]">A<sub>LCCA</sub></p>
          <n-input-number
            v-model:value="aLCCA"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'">mm<sup>2</sup></span>
            <span v-if="unit === 'squarecm'">cm<sup>2</sup></span>
            <span v-if="unit === 'squarein'">in<sup>2</sup></span>
          </p>
        </div>

        <div
          class="flex flex-row items-center space-x-4 my-4"
          v-if="mode === 'infarctHeart'"
        >
          <p class="text-xl font-medium w-[120px]">A<sub>RCA</sub></p>
          <n-input-number
            v-model:value="aRCA"
            clearable
            :placeholder="placeholder"
            size="large"
            :on-change="hideOutput"
          />
          <p class="text-lg font-normal w-[50px]">
            <span v-if="unit === 'squaremm'">mm<sup>2</sup></span>
            <span v-if="unit === 'squarecm'">cm<sup>2</sup></span>
            <span v-if="unit === 'squarein'">in<sup>2</sup></span>
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
        The percentage of infarcted
        {{ mode === "infarctArtery" ? "artery" : "heart" }} is
        {{ output.val }} %
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
          The percentage of infarcted artery is calculated based on the xx model
          <a
            href="https://doi.org/10.4244/EIJV7I11A206"
            target="_blank"
            rel="noopener"
            class="text-blue-400 hover:text-blue-600 transition-all hover:underline"
          >
            [1]
          </a>
        </p>
        <p class="reference" v-if="mode == 'infarctHeart'">
          The percentage of infarcted heart is calculated based on the xx model
          <a
            href="https://doi.org/10.4244/eijv11sva3"
            target="_blank"
            rel="noopener"
            class="text-blue-400 hover:text-blue-600 transition-all hover:underline"
          >
            [2]
          </a>
        </p>

        <div
          class="flex flex-col space-y-8 items-center"
          v-if="mode == 'infarctArtery'"
        >
          <span v-html="infarctArteryEq1"></span>
          <span v-html="infarctArteryEq2"></span>

          <div class="h-[2px] w-full my-4 bg-black"></div>

          <p class="text-2xl md:text-3xl font-semibold">
            %Infarct<sub>artery</sub> = {{ output.val }} %
          </p>
        </div>

        <div
          class="flex flex-col space-y-8 items-center"
          v-if="mode == 'infarctHeart'"
        >
          <span v-html="infarctHeartEq1"></span>
          <span v-html="infarctHeartEq2"></span>

          <p class="text-2xl md:text-3xl font-semibold">
            %Infarct<sub>heart</sub> = {{ output.val }} %
          </p>
        </div>
      </div>
    </n-collapse-transition>
  </main>
</template>

<script setup lang="ts">
import katex from "katex";
import "katex/dist/katex.css";

const infarctArteryEq1 = ref("");
const infarctArteryEq2 = ref("");

const infarctHeartEq1 = ref("");
const infarctHeartEq2 = ref("");

onMounted(() => {
  infarctArteryEq1.value = katex.renderToString(
    "\\% Infarct_{heart} = \\left(\\frac{A_{SB}^{\\frac{4}{3}}}{A_{LCCA}^{\\frac{4}{3}} + A_{RCA}^{\\frac{4}{3}}}\\right) \\times 100",
    {
      throwOnError: true,
    }
  );
});

const mode = ref("infarctArtery");
const unit = ref("squaremm");

const aMA = ref();
const aSB = ref();
const aLCCA = ref();
const aRCA = ref();

const showOutput = ref(false);

const output = ref({
  label: "",
  val: "",
});

const resetCalculation = (value: string) => {
  showOutput.value = false;

  if (value === "infarctArtery") {
    aMA.value = null;
    aSB.value = null;
  } else if (value === "infarctHeart") {
    aSB.value = null;
    aLCCA.value = null;
    aRCA.value = null;
  }

  mode.value = value;
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
    if (emptyInput(aMA.value) || emptyInput(aSB.value)) {
      return true;
    }
  } else if (mode.value === "infarctHeart") {
    if (
      emptyInput(aSB.value) ||
      emptyInput(aLCCA.value) ||
      emptyInput(aRCA.value)
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
    aMA: aMA.value,
    aSB: aSB.value,
    aLCCA: aLCCA.value,
    aRCA: aRCA.value,
  });

  if (mode.value === "infarctArtery") {
    const n1 = aSB.value / aMA.value;
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

    infarctArteryEq2.value = katex.renderToString(
      "\\% Infarct_{artery} = \\left(\\frac{" +
        aSB.value +
        "}{" +
        aMA.value +
        "}\\right)^{\\frac{4}{3}} \\times 100",
      {
        throwOnError: false,
      }
    );
  } else if (mode.value === "infarctHeart") {
    const n1 = Math.pow(aSB.value, 4 / 3);
    const n2 = Math.pow(aLCCA.value, 4 / 3);
    const n3 = Math.pow(aRCA.value, 4 / 3);

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

    infarctHeartEq2.value = katex.renderToString(
      "\\% Infarct_{heart} = \\left(\\frac{" +
        aSB.value +
        "^{\\frac{4}{3}}}{(" +
        aLCCA.value +
        "^{\\frac{4}{3}} + " +
        aRCA.value +
        "^{\\frac{4}{3}})}\\right) \\times 100",
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
