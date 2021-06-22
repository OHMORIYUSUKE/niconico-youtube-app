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
        <v-row>
          <v-col sm="8">
            <iframe
              width="100%"
              height="415"
              :src="
                'https://www.youtube.com/embed/' +
                $route.params.id +
                '?autoplay=1&mute=0&loop=1&playlist=' +
                vlist
              "
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
            <p class="text-h6">{{ videoTitle }}</p>

            <p>{{ publishTime }} 公開</p>

            <v-divider></v-divider>

            <v-row class="mb-3 mt-3">
              <v-col sm="1">
                <v-avatar :color="userIconColor" size="48">
                  <span class="white--text text-h6">{{
                    channelTitle.substring(0, 2)
                  }}</span>
                </v-avatar>
              </v-col>
              <v-col sm="11">
                <p class="mt-2 font-weight-medium channelTitle">
                  {{ channelTitle }}
                </p>
                <p class="mt-1">{{ videoDescription }}</p>
              </v-col>
            </v-row>
            <v-divider></v-divider>
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

export default {
  name: "Video",
  data: () => ({
    articles: [],
    vlist: [],
    userIconColor: String,
    videoTitle: String,
    videoDescription: String,
    channelTitle: String,
    publishTime: String,
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

    // 連続再生list生成
    const videoIds = [];
    for (let i = 0; i < 100; i++) {
      videoIds.push(this.$route.params.id);
    }
    this.vlist = videoIds;
    //

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
    this.channelTitle = title.snippet.channelTitle;

    // --
    const D = new Date(title.snippet.publishTime);
    const y = D.getFullYear();
    const month = ("00" + (D.getMonth() + 1)).slice(-2);
    const d = ("00" + D.getDate()).slice(-2);

    const updatedAt = y + "/" + month + "/" + d;

    // --
    this.publishTime = updatedAt;
    //
    // ユーザーアイコン色
    const colorlist = [
      "red",
      "purple",
      "green",
      "teal",
      "deep-orange",
      "light-blue",
    ];
    const day = D.getDate();
    if (1 <= day && day <= 5) {
      this.userIconColor = colorlist[0];
    } else if (6 <= day && day <= 10) {
      this.userIconColor = colorlist[1];
    } else if (11 <= day && day <= 15) {
      this.userIconColor = colorlist[2];
    } else if (16 <= day && day <= 20) {
      this.userIconColor = colorlist[3];
    } else if (21 <= day && day <= 25) {
      this.userIconColor = colorlist[4];
    } else {
      this.userIconColor = colorlist[5];
    }
  },
  methods: {
    search_video: function () {
      //console.log('検索！！');
      const ta3 = document.getElementById("searchTextId").value;
      console.log(ta3);
      this.$router.push("/?q=" + ta3);
    },
  },
  watch: {
    $route(to) {
      //遷移先のpathを取得
      // 連続再生list生成
      const videoIds = [];
      for (let i = 0; i < 100; i++) {
        videoIds.push(this.$route.params.id);
      }
      this.vlist = videoIds;
      //---------------

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
      this.channelTitle = title.snippet.channelTitle;
      //
      // --
      const D = new Date(title.snippet.publishTime);
      const y = D.getFullYear();
      const month = ("00" + (D.getMonth() + 1)).slice(-2);
      const d = ("00" + D.getDate()).slice(-2);

      const updatedAt = y + "/" + month + "/" + d;

      // --
      this.publishTime = updatedAt;

      // ユーザーアイコン色
      const colorlist = [
        "red",
        "purple",
        "green",
        "teal",
        "deep-orange",
        "light-blue",
      ];
      const day = D.getDate();
      if (1 <= day && day <= 5) {
        this.userIconColor = colorlist[0];
      } else if (6 <= day && day <= 10) {
        this.userIconColor = colorlist[1];
      } else if (11 <= day && day <= 15) {
        this.userIconColor = colorlist[2];
      } else if (16 <= day && day <= 20) {
        this.userIconColor = colorlist[3];
      } else if (21 <= day && day <= 25) {
        this.userIconColor = colorlist[4];
      } else {
        this.userIconColor = colorlist[5];
      }

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
.channelTitle {
  font-size: 18px;
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
