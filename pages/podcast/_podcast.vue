<template>
    <div class="min-h-screen mx-auto flex">
        <div class="content max-w-screen-sm mx-auto w-full py-16 px-4 sm:px-2">
            <PodcastDetails :podcastId="$route.params.podcast" />
        </div>
    </div>
</template>

<script>
import PodcastDetails from '~/components/PodcastDetails.vue'

export default {
    components: {
        PodcastDetails
    },
}
</script>

<style>

</style>
