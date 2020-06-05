<template>
    <div class="flex justify-end py-4">
      <div class="w-full sm:w-1/2 md:w-1/3 lg:w-1/4 px-4">
        <MultiSelect :options="categories" />
      </div>
      <div class="w-full sm:w-1/2 md:w-1/3 lg:w-1/4 px-2">
        <div class="flex items-center border-b border-b-2 border-teal-500 py-2">
          <input class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none" type="text" placeholder="Search" aria-label="Search">
        </div>
      </div>
    </div>
</template>

<script>
import categoriesList from '~/assets/categories.js'
import MultiSelect from './MultiSelect.vue';

export default {
    name: 'FilterList',
    components: {
      MultiSelect
    },
    data: () => ({
      categories: categoriesList
    })
}
</script>

<style>

</style>
