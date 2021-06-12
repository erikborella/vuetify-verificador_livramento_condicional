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
