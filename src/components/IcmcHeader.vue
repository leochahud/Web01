<template>
   <div>
       <v-app-bar color="white" class="overflow-hidden pa-5" min-height="100">
       <v-app-bar-title>Busca de Teses</v-app-bar-title>
       <router-link class="ma-3" to="/">Home</router-link>
       <router-link class="ma-3" to="/Grupo">Grupo</router-link>
       <v-spacer></v-spacer>
       <v-img v-bind:src="icmcLogo" class="mx-1" contain max-height="200" max-width="200"></v-img>
   </v-app-bar>
   </div>
</template>

<script>
export default {
  data () {
    return {
      title: 'ICMC Header',
      icmcLogo: require('@/assets/icmc-logo.png')
    }
  }
}
</script>
