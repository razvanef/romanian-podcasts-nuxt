<template>
    <div>
        <Header />
        <nuxt/>
        <CookiesConsent />
    </div>
</template>

<script>
import Header from '~/components/Header.vue'
import CookiesConsent from "~/components/CookiesConsent";

export default {
  layout: 'blog',
  components: {
    Header,
    CookiesConsent
  }
}
</script>