<template>
  <div class="container">
    <Header />
    <div class="content w-full pt-16 px-4 sm:px-2">
      <FilterList @filterListByQuery="filterQuery = $event" v-model="categoriesQuery" />
      <PodcastsList :searchPodcast="filterQuery" :selectCategories="categoriesQuery" />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Header from '~/components/Header.vue'
import PodcastsList from '~/components/PodcastsList.vue'
import FilterList from '~/components/FilterList.vue'

export default Vue.extend({
  components: {
    Header,
    PodcastsList,
    FilterList
  },
  data: () => ({
    filterQuery: '',
    categoriesQuery: []
  })
})
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
