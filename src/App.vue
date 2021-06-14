<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-icon class="pr-4">mdi-scale-balance</v-icon>
      
      <v-app-bar-title>
        Livramento Condicional
      </v-app-bar-title>
    </v-app-bar>

    <v-main>
      <!-- <HelloWorld/> -->
      <VerificadorLivramentoCondicional/>
    </v-main>

    <v-footer color="primary darken-1">
      <v-toolbar color="primary darken-1" flat dense>
        <v-toolbar-title class="text-body-1 white--text">
          {{ new Date().getFullYear() }} - <strong>Erik Borela</strong>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn icon target="_blank" href="https://github.com/erikborella/vuetify-verificador_livramento_condicional">
          <v-icon color="white">mdi-github</v-icon>
      </v-btn>
      </v-toolbar>
    </v-footer>
  </v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld';
  import VerificadorLivramentoCondicional from './components/VerificadorLivramentoCondicional.vue';

export default {
  name: 'App',

  components: {
    // HelloWorld,
    VerificadorLivramentoCondicional,
  },

  data: () => ({
    //
  }),
};
</script>
