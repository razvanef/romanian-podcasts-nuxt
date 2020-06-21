<template>
    <div class="min-h-screen mx-auto flex">
        <div class="content max-w-screen-sm mx-auto w-full py-16 px-4 sm:px-2">
            <SkeletonPage v-if="loading" />
            <PodcastDetails v-if="!loading" :podcast="podcast" />
        </div>
    </div>
</template>

<script>
import PodcastDetails from '~/components/PodcastDetails.vue'
import SkeletonPage from '~/components/SkeletonPage.vue'

export default {
    head() {
        const podcast = this.podcast;

        return {
        title: podcast.name,
        meta: [
            { name: 'description', content: podcast.description},
            { property: 'og:title', content: podcast.name},
            { property: 'og:site_name', content: "Podcasturi Romanesti"},
            { property: 'og:type', content: 'website'}
        ]
        }
    },
    components: {
        PodcastDetails,
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
            if (!this.$route.params.podcast) return;
            this.loading = true;
            
            fetch(
                `https://api.airtable.com/v0/appat34KlYh94IXEb/Podcasts?maxRecords=1&view=Grid%20view&filterByFormula=podcastId='${this.$route.params.podcast}'`,
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
