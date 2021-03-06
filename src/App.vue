<template>
  <v-app>
    <v-navigation-drawer
      app
      v-model="drawer"
      absolute
      class="teal darken-2 fill-height"
      dark
      temporary
    >
      <v-list-item>
        <v-list-item-icon>
          <v-icon>{{"mdi-monitor-dashboard"}}</v-icon>
        </v-list-item-icon>

        <v-list-item-content>
          <v-list-item-title>Genre</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-divider></v-divider>
      <v-list dense>
        <v-list-item-group
          v-model="item"
          color="white"
        >
          <v-list-item
            v-for="item in items"
            :key="item.title"
            link
          >
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app color="teal lighten-1" dense dark>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Spin the Movie</v-toolbar-title>

      <v-spacer></v-spacer>
    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container class="fill-height" fluid>
        <v-row justify="center" align="center">
          <v-col cols="12" sm="12" md="12" lg="6" v-bind:style="{ textAlign: 'center' }">
            <FortuneWheel
              :key="item"
              style="width: 100%, height: 100%"
              borderColor="#584b43"
              :fontSize="20"
              :btnWidth="80"
              :textLength="20"
              :borderWidth="6"
              :prizes="getPrizes"
              :verify="isVerifiedCanvas"
              @rotateStart="onCanvasRotateStart"
              @rotateEnd="onRotateEnd"
            />
          </v-col>
          <v-col cols="12" sm="12" md="12" lg="6">
            <movie-details :movieData="movie"></movie-details>
          </v-col>
        </v-row>
        <!-- If using vue-router -->
        <!-- <router-view></router-view> -->
      </v-container>
    </v-main>

    <v-footer
      dark
      padless
    >
      <v-card
        class="flex"
        flat
        tile
      >

        <v-card-text class="py-2 white--text text-center">
          {{ new Date().getFullYear() }} — <strong>Made with ❤️ by pavilandoangelo</strong>
          <v-spacer></v-spacer>
          <v-btn
            v-for="conn in connections"
            :key="conn.mdiIcon"
            class="mx-4"
            dark
            icon
            @click="openConnectionLink(conn)"
          >
            <v-icon size="24px">
              {{ conn.mdiIcon }}
            </v-icon>
          </v-btn>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>

import MovieDetails from "./components/MovieDetails";
import FortuneWheel from "vue-fortune-wheel";
import "vue-fortune-wheel/lib/vue-fortune-wheel.css";
import movies from "./data/movies.json"

export default {
  name: 'App',
  components: {
    FortuneWheel,
    MovieDetails,
  },

  data: () => ({
    drawer: false,
    item: 0,
    items: movies,
    canvasVerify: true,
    prizes: [],
    movie: true,
    connections: [
      {
        "mdiIcon": "mdi-facebook",
        "link": "https://www.facebook.com/pavilandoangelo/"
      },
      {
        "mdiIcon": "mdi-linkedin",
        "link": "https://www.linkedin.com/in/angelo-pavilando-2894a7137/?fbclid=IwAR3BSJZ0q4jqXyxyVuP8l10DkFoG3toa3PUNKJl55Yvoq18TtOIKKWGSl0A"
      },
      {
        "mdiIcon": "mdi-github",
        "link": "https://github.com/pavilandoangelo"
      }
    ],
  }),
  created() {
    // console.log("movies >> > ", movies);
    const data = movies[this.item].data.map(movie => ({...movie, bgColor: this.getRandomColor()}));
    // console.log("data >>> ", data);
    this.prizes = data;
    // console.log("this.prizes >>> ", this.prizes);
  },
  computed: {
    isVerifiedCanvas: function () {
      return this.canvasVerify;
    },
    getIcons: function () {
      return this.icons;
    },
    getPrizes: function () {
      return this.prizes;
    },
  },
  methods: {
    onImageRotateStart() {
      console.log("onRotateStart");
    },
    onCanvasRotateStart(rotate) {
      if (this.isVerifiedCanvas) {
        const verified = true;
        this.DoServiceVerify(verified, 1).then((verifiedRes) => {
          if (verifiedRes) {
            console.log("Verification passed, start to rotate");
            rotate();
            this.canvasVerify = false;
          } else {
            console.log("Failed verification");
          }
        });
        return;
      }
      console.log("onCanvasRotateStart");
    },
    onRotateEnd(prize) {
      this.getMovieData(prize.name);
    },
    DoServiceVerify(verified, duration) {
      return new Promise((resove) => {
        setTimeout(() => {
          resove(verified);
        }, duration);
      });
    },
    getRandomColor() {
      let letters = '0123456789ABCDEF';
      let color = '#';
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    getMovieData(movie) {
      const baseURI = `https://www.omdbapi.com/?apikey=b0742a8e&t=${movie}`
      this.$http.get(baseURI)
      .then((result) => {
        this.movie = result.data
        console.log("result >>> ", result);
        console.log("this.movie >>> ", this.movie);
      })
    },
    openConnectionLink(connection) {
      window.open(connection.link, "_blank")
    }
  },
  watch: {
    item(newValue) {
      this.prizes = movies[newValue].data.map(movie => ({...movie, bgColor: this.getRandomColor()}));
    },
  }
};
</script>

<style>
  .fortuneWheel-btn {
    font-size: 1.75rem !important;
    font-size: clamp(1.75rem, 1.75rem, 2rem);
  }
</style>