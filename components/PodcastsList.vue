<template>
  <div class="flex flex-wrap">
    <div v-if="loading"  class="flex w-full flex-wrap">
      <div v-for="i in 16" :key="i" class="flex w-full sm:w-1/2 md:w-1/3 lg:w-1/4">
        <SkeletonCard />
      </div>
    </div>
    <div class="flex w-full sm:w-1/2 md:w-1/3 lg:w-1/4"
      v-for="podcast in filterPodcasts"
      :key="podcast.id">
      <div class="flex-grow rounded overflow-hidden shadow-xl m-4 hover:shadow-round cursor-pointer">
        <nuxt-link :to="'podcast/'+podcast.fields.podcastId">
          <img class="w-full" :src="podcast.fields.cover" alt="Sunset in the mountains">
          <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{podcast.fields.name}}</div>
            <p class="text-gray-700 text-base">
              {{podcast.fields.host}}
            </p>
          </div>
        </nuxt-link>
      </div>
    </div>
    <p v-if="!loading && filterPodcasts.length === 0" class="m-auto mt-8">No podcasts</p>
  </div>
</template>

<script>
import categoriesList from "../assets/categories.js";
import SkeletonCard from '~/components/SkeletonCard.vue'

export default {
  name: "PodcastsList",
  props: ['searchPodcast', 'selectCategories', 'podcasts'],
  components: {
    SkeletonCard
  },
  data: () => ({
    title: "Podcast-uri romanesti",
    categories: categoriesList
  }),
  computed: {
    filterPodcasts() {
      const search = this.searchPodcast
        ? this.searchPodcast.normalize("NFD").replace(/[\u0300-\u036f]/g, "")
        : "";
      const list = this.podcasts;

      return list.filter(podcast => {
        return (
          !!(
            podcast.fields.name
              .toLowerCase()
              .normalize("NFD")
              .replace(/[\u0300-\u036f]/g, "")
              .includes(search.toLowerCase()) ||
            podcast.fields.host
              .toLowerCase()
              .normalize("NFD")
              .replace(/[\u0300-\u036f]/g, "")
              .includes(search.toLowerCase())
          ) &&
          !!(this.selectCategories.length > 0
            ? podcast.fields.categories.filter(value =>
                this.selectCategories.some(categorie => categorie.value === value)
              ).length > 0
            : true)
        );
      });
    }
  },
  methods: {
    track () {
      this.$ga.page('/')
    }
  },
};
</script>
<style></style>
