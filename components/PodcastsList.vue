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
  props: ['searchPodcast', 'selectCategories'],
  components: {
    SkeletonCard
  },
  data: () => ({
    title: "Podcast-uri romanesti",
    loading: false,
    podcasts: [],
    categories: categoriesList
  }),
  created() {
    this.listPodcasts();
    this.loading = true;
  },
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
    listPodcasts(offset = "") {
      const fields =
        "&fields%5B%5D=podcastId&fields%5B%5D=name&fields%5B%5D=host&fields%5B%5D=cover&fields%5B%5D=description&fields%5B%5D=categories";
      fetch(
        `https://api.airtable.com/v0/appat34KlYh94IXEb/Podcasts?view=Grid%20view&offset=${offset}${fields}`,
        {
          headers: {
            Authorization: `Bearer ${process.env.AIRTABLE_API_KEY}`
          }
        }
      )
        .then(res => res.json())
        .then(json => {
          this.loading = false;
          this.podcasts = [...this.podcasts, ...json.records];
          if (json.offset) this.listPodcasts(json.offset);
        })
        .catch(err => {
          
        });
    },
    
    track () {
      this.$ga.page('/')
    }
  },
};
</script>
<style></style>
