<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

      </div>

      <v-spacer class="center">Zaawansowane bazy danych i hurtownie danych</v-spacer>

      <v-btn
        target="_blank"
        text
      >
      </v-btn>
    </v-app-bar>

    <v-content>
      <Chart/>
    </v-content>
  </v-app>
</template>

<script>
import Chart from './components/Chart';

export default {
  name: 'App',

  components: {
    Chart,
  },

  data: () => ({
    //
  }),
};
</script>
<style scoped>
.center {
  position: relative;
          left: 35%;
          font-size: 24px;

}
</style>
