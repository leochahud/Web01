<template>
  <v-app id="app">
    <icmcHeader></icmcHeader>
    <router-view></router-view>
    <discFooter></discFooter>
  </v-app>
</template>

<script>
import IcmcHeader from './components/IcmcHeader.vue'
import DiscFooter from './components/DiscFooter.vue'
export default {
  name: 'App',

  components: {
    icmcHeader: IcmcHeader,
    discFooter: DiscFooter
  },

  data: () => ({
    //
  })
}
</script>

<style scoped>
  div[data-app='true'] {
    background-image: url("@/assets/icmc-wallpaper.jpg");
    background-size: cover;
    background-position: center center;
    background-attachment: fixed;
  }
</style>
