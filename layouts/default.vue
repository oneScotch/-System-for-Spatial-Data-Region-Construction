<template>
  <v-app>
    <v-main style="background-color:#fbfbfc">
      <v-container>
        <TheNavBar />
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import TheNavBar from '@/components/TheNavBar'

export default {
  components: {
    TheNavBar,
  },
}
</script>

<style>
#app {
  font-family: 'Open Sans', sans-serif;
}
</style>
