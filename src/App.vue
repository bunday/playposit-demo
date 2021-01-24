<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h3>Interactive 360 Video Demo</h3>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://zadatolayinka.dev/"
        target="_blank"
        text
      >
        <span class="mr-2">by Zadat Olayinka</span>
        <v-icon>mdi-person</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
