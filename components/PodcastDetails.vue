<template>
    <div class="text-gray-700 text-left">
        <SkeletonPage v-if="loading" />
        <div v-if="!loading">
            <div class="p-10 max-w-md mx-auto">
                <img :src="podcast.cover" />
            </div>
            <div class="text-2xl pb-2 text-black">
                {{podcast.name}}
            </div>
            <div class="border-b border-gray-400 text-sm pb-4">
                {{podcast.host}}
            </div>
            <div class="border-b border-gray-400 py-4">
                {{podcast.description}}
            </div>
            <div class="border-b border-gray-400 py-4">
                <p class="text-xl mb-2">Asculta:</p>
                <ListenButtons :podcast="podcast" />
            </div>
            <div class="flex text-sm pt-4">
                <div class="border border-gray-500 rounded-full m-2 px-4 py-1" 
                    v-for="(category, index) in podcast.categories"
                    :key="index">
                    {{ category }}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import ListenButtons from "~/components/ListenButtons";
import SkeletonPage from '~/components/SkeletonPage.vue'

export default {
    props: ['podcastId'],
    components: {
        ListenButtons,
        SkeletonPage
    },
    created() {
        this.getPodcast();
    },
    data: () => ({
        podcast: {},
        loading: false
    }),
    methods: {
        getPodcast() {
            this.loading = true;
            if (!this.podcastId) return;
            this.loading = true;

            fetch(
                `https://api.airtable.com/v0/appat34KlYh94IXEb/Podcasts?maxRecords=1&view=Grid%20view&filterByFormula=podcastId='${this.podcastId}'`,
                {
                headers: {
                    Authorization: `Bearer ${process.env.AIRTABLE_API_KEY}`
                }
                }
            )
                .then(res => res.json())
                .then(json => {
                    this.podcast = json.records[0].fields;
                    this.loading = false;
                });
                }
            }
}
</script>

<style>

</style>
