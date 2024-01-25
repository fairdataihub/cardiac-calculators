<template>
  <main
    class="text-gray-800 h-screen px-4 py-8 md:py-0 mx-auto max-w-screen-md flex flex-col justify-center items-center my-auto"
  >
    <div>
      <h2 class="mb-4 text-4xl font-bold text-center text-gray-900">
        Contact Us
      </h2>
      <p class="mb-8 text-center text-gray-500 sm:text-xl">
        If you have any issues with this website you can open an issue on our
        GitHub repository. We will try to resolve the issue as soon as possible.
      </p>

      <n-space align="center" justify="center">
        <NuxtLink
          to="https://github.com/fairdataihub/cardiac-calculators/issues/new/choose"
          target="__blank"
        >
          <n-button size="large" type="warning" secondary
            >I have an issue</n-button
          >
        </NuxtLink>
      </n-space>
    </div>
  </main>
</template>
