<template>
  <v-app>
   

    <v-content>
      <VueForm/>
    </v-content>
  </v-app>
</template>

<script>
import VueForm from './components/VueForm';

export default {
  name: 'App',

  components: {
    VueForm,
  },

  data: () => ({
    //
  }),
};
</script>
