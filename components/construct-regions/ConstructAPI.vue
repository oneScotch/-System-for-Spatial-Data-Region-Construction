<template>
  <v-menu transition="slide-y-transition">
    <template v-slot:activator="{ on, attrs }">
      <v-btn color="primary"
             class="ma-2"
             v-bind="attrs"
             v-on="on">
        Constructing Meaningful Regions for Data Exploration
      </v-btn>
    </template>
    <v-list>
      <v-list-item v-for="n in algorithms"
                   :key="n"
                   link>
        <v-list-item-title v-text="n"></v-list-item-title>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script>


export default {
  components: {
  },
  data() {
    return {
      algorithms:["GEA","GMA","k-means","DBSCAN"],
    }
  },

  fetch() {
  },


  methods: {
  },
}
</script>
