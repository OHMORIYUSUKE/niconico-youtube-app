<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ on, attrs }">
        <v-app-bar-nav-icon v-bind="attrs" v-on="on"></v-app-bar-nav-icon>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2"> MyTube </v-card-title>

        <v-card-text>
          <p>YouTubeもどきです。</p>
          <p>APIのコール数が上限に達すると利用できなくなります。</p>
          <a href="https://twitter.com/uutan1108">作った人</a>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false"> 閉じる </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
    };
  },
};
</script>

<style scoped></style>
