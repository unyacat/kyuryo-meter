<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      flat
    >
      <v-toolbar-title>
        <v-icon>
          mdi-clock-time-five-outline
        </v-icon>
        秒給メーター
      </v-toolbar-title>

    </v-app-bar>

    <v-main>
      <kyuryo />
    </v-main>
  </v-app>
</template>

<script>
import kyuryo from './components/Kyuryo.vue';

export default {
  name: 'App',

  components: {
    kyuryo
  },
  data: () => ({
    //
  }),
};
</script>
