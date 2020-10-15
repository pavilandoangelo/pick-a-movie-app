<template>
  <v-app>
    <v-navigation-drawer
      app
      v-model="drawer"
      absolute
      class="blue fill-height"
      dark
    >
      <v-list-item>
        <v-list-item-avatar>
          <v-img src="https://randomuser.me/api/portraits/men/78.jpg"></v-img>
        </v-list-item-avatar>

        <v-list-item-content>
          <v-list-item-title>John Leider</v-list-item-title>
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

    <v-app-bar app color="primary" dense dark>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Spin the Movie</v-toolbar-title>

      <v-spacer></v-spacer>
    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>

      <!-- Provides the application the proper gutter -->
      <v-container fluid>
        <v-row justify="center" align="center">
          <v-col md="7" v-bind:style="{ textAlign: 'center' }">
            <FortuneWheel
              style="width: 70%"
              borderColor="#584b43"
              :fontSize="16"
              :textLength="20"
              :borderWidth="6"
              :prizes="prizes"
              :verify="isVerifiedCanvas"
              @rotateStart="onCanvasRotateStart"
              @rotateEnd="onRotateEnd"
            />
          </v-col>
          <v-col md="5">
            <movie-details :movieData="movie"></movie-details>
          </v-col>
        </v-row>
        <!-- If using vue-router -->
        <!-- <router-view></router-view> -->
      </v-container>
    </v-main>

    <!-- <v-footer app>
    </v-footer> -->
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
    drawer: true,
    item: -1,
    items: [
      { title: 'Home', icon: 'mdi-view-dashboard' },
      { title: 'About', icon: 'mdi-forum' },
    ],
    canvasVerify: true,
    prizes: [],
    movie: true,
  }),
  created() {
    // console.log("movies >>> ", movies);
    const data = movies.bots.map(movie => ({...movie, bgColor: this.getRandomColor()}));
    // console.log("data >>> ", data);
    this.prizes = data;
    // console.log("this.prizes >>> ", this.prizes);
  },
  computed: {
    isVerifiedCanvas: function () {
      return this.canvasVerify;
    },
  },
  methods: {
    onImageRotateStart() {
      console.log("onRotateStart");
    },
    onCanvasRotateStart(rotate) {
      console.log(rotate);
      console.log(this.isVerifiedCanvas);
      if (this.isVerifiedCanvas) {
        const verified = true;
        this.DoServiceVerify(verified, 1).then((verifiedRes) => {
          if (verifiedRes) {
            console.log("Verification passed, start to rotate");
            rotate();
            this.canvasVerify = false;
          } else {
            alert("Failed verification");
          }
        });
        return;
      }
      console.log("onCanvasRotateStart");
    },
    onRotateEnd(prize) {
      this.getMovieData(prize.value);
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
    }
  },
};
</script>
