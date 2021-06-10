<template>
  <v-app>
    <v-app-bar
      fixed
      elevate-on-scroll
      color="white"
      scroll-target="#scrolling-techniques-7"
    >
      <HelloWorld></HelloWorld>

      <v-toolbar-title
        ><v-icon large="true" color="rgb(255, 19, 19)">mdi-youtube</v-icon>
        MyTube</v-toolbar-title
      >

      <v-spacer> </v-spacer>

      <v-text-field
        class="text"
        id="searchTextId"
        v-model="searchText"
        hide-details
        single-line
        outlined
      ></v-text-field>

      <v-btn depressed v-on:click="search_video()">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-spacer> </v-spacer>
    </v-app-bar>

    <div class="maincontent">
      <v-container>
        <v-row no-gutters>
          <div v-if="articles == false">
            <h3>取得できませんでした</h3>
          </div>
          <div v-else>
            <div>
              <v-col
                v-for="article in articles"
                :key="article.id.videoId"
                sm="4"
              >
                <div class="card" elevation="1">
                  <iframe
                    width="100%"
                    height="215"
                    :src="'https://www.youtube.com/embed/' + article.id.videoId"
                    title="YouTube video player"
                    frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                    allowfullscreen
                  ></iframe>
                </div>
              </v-col>
            </div>
          </div>
        </v-row>
      </v-container>
    </div>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import axios from "axios";
import Vue from "vue";

// データオブジェクト
var data = { a: "アイマス" };

// Vue インスタンスにオブジェクトを追加する
var vm = new Vue({
  data: data,
});
console.log(data.a);

export default {
  name: "Home",
  data: () => ({
    articles: [],
    error: String,
  }),
  components: {
    HelloWorld,
  },
  async mounted() {
    // 記事を取得する
    console.log("mounted()" + data.a);
    const response = await axios
      .get(
        "https://www.googleapis.com/youtube/v3/search?type=video&part=snippet&q=" +
          data.a +
          "&maxResults=15&key=" +
          process.env.VUE_APP_API_KEY //直す
      )
      .catch((error) => {
        //window.alert("取得に失敗しました。\n" + error);
        console.log(error);
      });
    this.articles = response.data.items;
    console.log(response.data.items);
  },
  methods: {
    search_video: async function () {
      const ta3 = document.getElementById("searchTextId").value;
      vm.a = ta3;
      console.log(vm.a);
      const response = await axios.get(
        "https://www.googleapis.com/youtube/v3/search?type=video&part=snippet&q=" +
          data.a +
          "&maxResults=15&key=" +
          process.env.VUE_APP_API_KEY //直す
      );
      console.log(response.data);
      this.articles = response.data.items;
    },
  },
};
</script>

<style scoped>
.maincontent {
  padding-top: 70px;
  background-color: rgb(245, 245, 245);
}
.leftcontent {
  margin-right: 15px;
}
.card {
  margin: 4px;
}
</style>
