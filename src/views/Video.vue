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
          MyTube</v-toolbar-title
        >
      </router-link>

      <v-spacer> </v-spacer>

      <v-text-field
        class="text"
        id="searchTextId"
        hide-details
        single-line
        outlined
        value="このページの検索機能は実装されていません。"
      ></v-text-field>

      <v-btn depressed>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-spacer> </v-spacer>
    </v-app-bar>

    <div class="maincontent">
      <v-container>
        <v-row>
          <v-col sm="8">
            <iframe
              width="100%"
              height="415"
              :src="
                'https://www.youtube.com/embed/' +
                $route.params.id +
                '?autoplay=1&mute=0'
              "
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
            <p class="text-h6">{{ videoTitle }}</p>
            <p>{{ videoDescription }}</p>
          </v-col>
          <v-col sm="4">
            <div v-for="article in articles" :key="article.id.videoId">
              <v-row>
                <v-col sm="7">
                  <router-link :to="'/video/' + article.id.videoId">
                  <figure>
                    <img
                      width="100%"
                      height="115"
                      :src="
                        'https://img.youtube.com/vi/' +
                        article.id.videoId +
                        '/hqdefault.jpg'
                      "
                      @click="$vuetify.goTo(0)"
                    />
                  </figure>
                  </router-link>
                </v-col>
                <v-col sm="5">
                  <p v-if="article.snippet.title.length < 35">
                    {{ article.snippet.title }}
                  </p>
                  <p v-else>
                    {{ article.snippet.title.substring(0, 35) + "..." }}
                  </p>
                </v-col>
              </v-row>
            </div>
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
  name: "Video",
  data: () => ({
    articles: [],
    videoTitle: String,
    videoDescription: String,
    error: String,
  }),
  components: {
    HelloWorld,
  },
  async mounted() {
    this.$vuetify.goTo(0);
    // 記事を取得する
    this.articles = JSON.parse(localStorage.getItem("videolist"));
    console.log(JSON.parse(localStorage.getItem("videolist")));
    //
    const JSONDATA = JSON.parse(localStorage.getItem("videolist"));
    const getFruitById = (id) => {
      const fruitIndex = JSONDATA.findIndex((data) => data.id.videoId === id);
      return JSONDATA[fruitIndex];
    };

    console.log(this.$route.params.id);
    const title = getFruitById(this.$route.params.id);
    console.log(title.snippet.title); // display title
    this.videoTitle = title.snippet.title;
    this.videoDescription = title.snippet.description;
    //
  },
  methods: {
    search_video: async function () {
      const ta3 = document.getElementById("searchTextId").value;
      vm.a = ta3;
      console.log(vm.a);
      const response = await axios.get(
        "https://www.googleapis.com/youtube/v3/search?type=video&part=snippet&q=" +
          data.a +
          "&maxResults=21&key=" +
          process.env.VUE_APP_API_KE //直す
      );
      console.log(response.data.items);
      localStorage.setItem("videolist", JSON.stringify(response.data.items));
      this.articles = response.data.items;
    },
  },
  watch: {
    $route(to) {
      //遷移先のpathを取得
      console.log("watch : " + to.path);
      console.log("watch :" + this.$route.params.id);
      //
      const JSONDATA = JSON.parse(localStorage.getItem("videolist"));
      const getFruitById = (id) => {
        const fruitIndex = JSONDATA.findIndex((data) => data.id.videoId === id);
        return JSONDATA[fruitIndex];
      };
      const title = getFruitById(this.$route.params.id);
      this.videoTitle = title.snippet.title;
      console.log(title.snippet.description);
      this.videoDescription = title.snippet.description;
      //
      this.detectPath(to.path);
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
  background: #fff;
  overflow: hidden;
}


figure img {
  -webkit-transform: scale(1);
  transform: scale(1);
  -webkit-transition: .3s ease-in-out;
  transition: .3s ease-in-out;
}
figure:hover img {
  -webkit-transform: scale(1.2);
  transform: scale(1.2);
}
</style>
