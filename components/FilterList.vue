<template>
    <div class="flex flex-wrap justify-end py-4">
      <div class="flex w-full sm:w-1/2 md:w-1/3 lg:w-1/4 px-4 mt-4">
        <MultiSelect :options="categories" @elementSelected="searchCategories = $event; $emit('input', searchCategories)" />
      </div>
      <div class="flex w-full sm:w-1/2 md:w-1/3 lg:w-1/4 px-4 mt-4">
        <div class="flex flex-grow items-center border-b border-b-2 border-teal-500 py-1">
          <input v-model="searchQuery" @input="$emit('filterListByQuery', searchQuery)"
            class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
            type="text" placeholder="Search" aria-label="Search">
          <div class="text-xl mr-2 cursor-pointer"
            v-if="searchQuery"
            @click.stop="clearSearch()">
            <fa :icon="['fas', 'times-circle']" />
          </div>
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
      categories: categoriesList,
      searchQuery: '',
      searchCategories: []
    }),
    methods: {
      clearSearch() {
        this.searchQuery = '';
        this.$emit('filterListByQuery', this.searchQuery)
      }
    }
}
</script>

<style>

</style>
