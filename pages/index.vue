<template>
  <main class="text-gray-800 mt-8 pb-20 flex-grow">
    <h1>Cardiac Calculators</h1>

    <p class="text-base md:text-lg font-normal mt-3 text-slate-600">
      Many mathematical formulas are used to guide decision making in
      cardiovascular healthcare. These include for instance formulas to
      calculate the optimal diameter of a stent or the myocardial mass at risk
      for infarct due to side branch occlusion. Here, we provide interactive
      calculators so that anyone can use these formulas easily and quickly
      without necessitating an understanding of the mathematical framework
      around them.
    </p>

    <div>
      <n-divider />
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <div class="bg-white shadow-md border border-slate-100 rounded-md p-4">
        <div class="flex justify-start space-x-8 items-center">
          <n-image width="150" src="/images/bifurcation-2.svg" />

          <n-space vertical>
            <h2 class="text-xl font-bold mb-4">Bi/Trifurcation Calculator</h2>
            <p class="mb-4">
              To calculate the optimal stent diameter of a diseased artery
              segment in a bifurcation or trifurcation.
            </p>
            <nuxt-link to="/bitrifurcation">
              <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                Go to Calculator
              </button>
            </nuxt-link>
          </n-space>
        </div>
      </div>

      <div class="bg-white shadow-md border border-slate-100 rounded-md p-4">
        <div class="flex justify-start space-x-8 items-center">
          <n-image width="150" src="/images/sketch.png" />

          <n-space vertical>
            <h2 class="text-xl font-bold mb-4">
              At-Risk Myocardial Mass Calculator
            </h2>
            <p class="mb-4">
              To calculate the myocardial mass at risk for infarct if a side
              branch is occluded.
            </p>
            <nuxt-link to="/myocardialinfarct">
              <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                Go to Calculator
              </button>
            </nuxt-link>
          </n-space>
        </div>
      </div>
    </div>
  </main>
</template>
