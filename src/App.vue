<template>
  <v-app id="inspire" white>
    <main>
      <router-view />
    </main>
  </v-app>
</template>

<script>
//import "../static/vendor/hamburgers/_sass/hamburgers/hamburgers.scss";
export default {
  data: () => ({
    drawer: false
  }),
  methods: {
    toggleDrawer: function(e) {
      this.drawer = !this.drawer;

      return;
    }
  },
  props: {
    source: String
  }
};
</script>
