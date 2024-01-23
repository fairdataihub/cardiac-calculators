<template>
  <div
    class="relative mx-auto flex h-screen w-full flex-col px-8"
    :class="{ 'debug-screens': devMode }"
  >
    <n-back-top />

    <nav class="bg-white border-gray-200">
      <div class="flex flex-wrap items-center justify-end mx-auto p-4">
        <button
          data-collapse-toggle="navbar-default"
          type="button"
          class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400"
          aria-controls="navbar-default"
          aria-expanded="false"
        >
          <span class="sr-only">Open main menu</span>
          <svg
            class="w-5 h-5"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 17 14"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M1 1h15M1 7h15M1 13h15"
            />
          </svg>
        </button>
        <div class="hidden w-full md:block md:w-auto" id="navbar-default">
          <ul
            class="font-medium flex flex-col p-4 md:p-0 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 rtl:space-x-reverse md:mt-0 md:border-0 md:bg-white"
          >
            <li>
              <a
                href="#"
                class="block py-2 px-3 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0"
                aria-current="page"
                >Home</a
              >
            </li>
            <li>
              <NuxtLink
                to="/bitrifurcation"
                class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0"
              >
                Bi/Trifurcation Calculator
              </NuxtLink>
            </li>
            <li>
              <NuxtLink
                to="/myocardialinfarct"
                class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0"
              >
                At-Risk Myocardial Mass Calculator
              </NuxtLink>
            </li>
            <li>
              <NuxtLink
                to="/contact"
                class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0"
              >
                Contact
              </NuxtLink>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <slot />

    <footer
      class="flex h-24 w-full items-center border-t bg-white p-4 flex-col md:flex-row md:items-center md:justify-between md:p-6"
    >
      <div
        class="flex flex-col items-center md:items-end text-sm font-medium text-slate-700"
      >
        <p class="text-center">Made with ❤️ by the FAIR Data Innovations Hub</p>
      </div>

      <div class="flex space-x-8 items-center mt-4 md:mt-0">
        <a
          href="https://fairdataihub.org/"
          target="_blank"
          rel="noopener"
          data-umami-event="fairdataihub-org-navigate"
        >
          <n-image
            width="140"
            src="https://fairdataihub.org/logo.svg"
            preview-disabled
          />
        </a>
        <a
          href="https://www.calmi2.org/"
          target="_blank"
          rel="noopener"
          data-umami-event="calmi2-org-navigate"
        >
          <n-image
            width="140"
            src="https://fairdataihub.org/images/collaborators/calmi-logo.png"
            preview-disabled
          />
        </a>
      </div>
    </footer>
  </div>
</template>

<script setup>
const devMode = process.env.NODE_ENV === "development";
</script>
