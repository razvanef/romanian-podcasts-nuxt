<template>
  <div class="container">
    <div class="content w-full pt-16 px-4 sm:px-2">
      <FilterList @filterListByQuery="filterQuery = $event" v-model="categoriesQuery" />
      <PodcastsList :podcasts="podcasts" :searchPodcast="filterQuery" :selectCategories="categoriesQuery" />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import PodcastsList from '~/components/PodcastsList.vue'
import FilterList from '~/components/FilterList.vue'

export default Vue.extend({
  async asyncData() {
    let loading = true;
    let podcasts = Array();
    let offset = '';
    const fields =
        "&fields%5B%5D=podcastId&fields%5B%5D=name&fields%5B%5D=host&fields%5B%5D=cover&fields%5B%5D=description&fields%5B%5D=categories";
    do {
      await fetch(
        `https://api.airtable.com/v0/appat34KlYh94IXEb/Podcasts?view=Grid%20view&offset=${offset}${fields}`,
        {
          headers: {
            Authorization: `Bearer ${process.env.NUXT_ENV_AIRTABLE_API_KEY}`
          }
        }
      )
      .then(res => res.json())
      .then(json => {
        loading = false;
        podcasts = [...podcasts, ...json.records];
        offset = "";
        if (json.offset) offset = json.offset;
      })
    } while(offset)

    return { podcasts }
      
  },
  head: {
    // if no subcomponents specify a metaInfo.title, this title will be used
    title: 'Podcasturi Romanesti',
    meta: [
        { name: 'description', content: "Ce podcasturi mai asculti? Am adunat o colectie cu cele mai populare podcasturi romanesti sa ai de unde alege." }
    ]
  },
  components: {
    PodcastsList,
    FilterList
  },
  data: () => ({
    filterQuery: '',
    categoriesQuery: [],
    loading: false,
    podcasts: Array(),
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
