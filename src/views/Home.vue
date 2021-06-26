<template>
  <v-app>
    <v-app-bar
      fixed
      elevate-on-scroll
      color="white"
      scroll-target="#scrolling-techniques-7"
    >
      <HelloWorld></HelloWorld>

      <router-link to="/" class="title">
        <v-toolbar-title
          ><v-icon large="true" color="rgb(255, 19, 19)">mdi-youtube</v-icon>
          <span v-show="$vuetify.breakpoint.smAndUp" class="ml-2"
            >MyTube</span
          ></v-toolbar-title
        >
      </router-link>

      <v-spacer> </v-spacer>

      <v-text-field
        class="text"
        id="searchTextId"
        v-model="searchText"
        hide-details
        single-line
        outlined
        dense
      ></v-text-field>

      <v-btn depressed v-on:click="search_video()">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-spacer> </v-spacer>
    </v-app-bar>

    <div class="maincontent">
      <v-container>
        <v-row v-if="articles == false">
          <v-col sm="4">
            <h3 class="errormsg">
              取得できませんでした。<br /><br />僕の動画でも見ていってね。<br />
            </h3>
            <iframe
              width="560"
              height="315"
              src="https://www.youtube.com/embed/UdRkqjRtTc0"
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </v-col>
        </v-row>
        <v-row no-gutters v-else>
          <v-col
            v-for="article in articles"
            :key="article.id.videoId"
            sm="3"
            cols="12"
            md="4"
            lg="3"
          >
            <router-link
              :to="'/video/' + article.id.videoId"
              style="text-decoration: none; color: black"
            >
              <div class="card" elevation="1">
                <figure>
                  <img
                    width="100%"
                    height="170"
                    :src="
                      'https://img.youtube.com/vi/' +
                      article.id.videoId +
                      '/hqdefault.jpg'
                    "
                  />
                </figure>
                <p>{{ article.snippet.title }}</p>
              </div>
            </router-link>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </v-app>
</template>

<script>
import HelloWorld from "../components/HelloWorld.vue";
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
    // ?q={}を取得する
    console.log(this.$route.query.q);
    // ?q={}があったら検索ワードを変える
    if (this.$route.query.q) {
      data.a = this.$route.query.q;
    }
    this.$router.push("/?q=" + data.a);
    const response = await axios
      .get(
        "https://www.googleapis.com/youtube/v3/search?type=video&part=snippet&q=" +
          data.a +
          "&maxResults=24&videoEmbeddable=true&key=" +
          process.env.VUE_APP_API_KEY //直す
      )
      .catch((error) => {
        //window.alert("取得に失敗しました。\n" + error);
        console.log(error);
      });
    this.articles = response.data.items;
    console.log(response.data.items);
    localStorage.setItem("videolist", JSON.stringify(response.data.items));
  },
  methods: {
    search_video: async function () {
      const ta3 = document.getElementById("searchTextId").value;
      vm.a = ta3;
      this.$router.push("/?q=" + ta3);
      console.log(vm.a);
      const response = await axios.get(
        "https://www.googleapis.com/youtube/v3/search?type=video&part=snippet&q=" +
          data.a +
          "&maxResults=24&videoEmbeddable=true&key=" +
          process.env.VUE_APP_API_KEY //直す
      );
      console.log(response.data.items);
      localStorage.setItem("videolist", JSON.stringify(response.data.items));
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
.errormsg {
  margin-right: 30px;
}
.title {
  text-decoration: none;
  color: black;
}
figure {
  margin: 0;
  padding: 0;
  background: rgb(245, 245, 245);
  overflow: hidden;
}
figure img {
  -webkit-transform: scale(1);
  transform: scale(1);
  -webkit-transition: 0.2s ease-in-out;
  transition: 0.2s ease-in-out;
}
figure:hover img {
  -webkit-transform: scale(1.05);
  transform: scale(1.05);
}
</style>
