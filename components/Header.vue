<template>
  <div class="flex z-20 bg-white border-b border-gray-200 fixed top-0 inset-x-0 z-100 h-16 items-center shadow-strong">
    <div class="flex w-full max-w-screen-xl relative mx-auto px-6">  
      <nuxt-link tag="a" to="/">
        <div class="d-flex align-center">
          <img
            alt="Podcasturi Romanesti"
            class="shrink mr-2"
            contain
            src="~/assets/images/podcasturi-romanesti-logo.png"
            width="180"
          />
        </div>
      </nuxt-link>

      <div class="flex-grow"></div>

      <button class="bg-white hover:bg-gray-100 focus:outline-none text-gray-800 font-semibold py-2 px-4 rounded items-center inline-flex"
        href="mailto:razvanef@gmail.com?subject=Podcasturi Romanesti"
        target="_blank"
      >
        <span class="mr-2">Ai o sugestie?</span>
        <div class="text-xl mr-2">
          <fa class="fill-current" :icon="['fas', 'envelope']" />
        </div>
      </button>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style></style>
