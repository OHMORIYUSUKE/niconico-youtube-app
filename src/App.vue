<template>
  <v-app>
<v-app-bar
      fixed
      elevate-on-scroll
      color="white"
      scroll-target="#scrolling-techniques-7"
    >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>MyTube</v-toolbar-title>

      <v-spacer>
      </v-spacer>

      <v-text-field
      class="text"
        v-model="searchText"
        hide-details single-line
        outlined></v-text-field>

    <v-btn depressed>
      <v-icon>mdi-magnify</v-icon>
    </v-btn>

      <v-spacer>
      </v-spacer>

    </v-app-bar>

    <div class="maincontent">
      <HelloWorld/>
    </div>
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

<style scoped>
.maincontent {
  padding-top: 70px;
  background-color: rgb(245, 245, 245);
}
</style>
