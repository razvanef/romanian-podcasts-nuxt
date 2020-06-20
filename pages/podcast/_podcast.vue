<template>
    <div class="container">
        <Header />
        <div class="content max-w-screen-sm py-16 px-4 sm:px-2">
            <PodcastDetails :podcastId="$route.params.podcast" />
        </div>
    </div>
</template>

<script>
import Header from '~/components/Header.vue'
import PodcastDetails from '~/components/PodcastDetails.vue'

export default {
    components: {
        Header,
        PodcastDetails
    },
}
</script>

<style>

</style>
