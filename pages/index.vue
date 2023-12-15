<template>
  <main class="text-gray-800 mt-8 pb-20 flex-grow">
    <h1>Cardiac Calculators</h1>

    <p class="text-base md:text-lg font-normal mt-3 text-slate-600">
      Explanatory text
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
              This calculator is used to determine the pressure drop across a
              bifurcation or trifurcation.
            </p>
            <nuxt-link to="/bitrifurcation">
              <button
                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
              >
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
              Percentage Infarct Calculator
            </h2>
            <p class="mb-4">
              This calculator is used to determine the percentage of myocardial
              infarct.
            </p>
            <nuxt-link to="/myocardialinfarct">
              <button
                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
              >
                Go to Calculator
              </button>
            </nuxt-link>
          </n-space>
        </div>
      </div>
    </div>
  </main>
</template>
