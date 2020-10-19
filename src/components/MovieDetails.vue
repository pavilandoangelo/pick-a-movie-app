<template>
  <v-card
    :loading="loading"
  >
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="3"
        indeterminate
      ></v-progress-linear>
    </template>
    <v-container fluid class="fill-height">
      <v-row>
        <v-col cols="5">
          <v-img
            :src="movie.Poster ? movie.Poster : this.offlineImage"
            :lazy-src="movie.Poster ? movie.Poster : this.offlineImage"
            height="400"
            width="300"
            aspect-ratio="1.7"
            contain
          >
            <template v-slot:placeholder>
              <v-row
                class="fill-height ma-0"
                align="center"
                justify="center"
              >
                <v-progress-circular
                  indeterminate
                  color="grey lighten-5"
                ></v-progress-circular>
              </v-row>
            </template>
          </v-img>
        </v-col>
        <v-col cols="6">
          <v-card-title class="align-start">
          <div>
            <span class="headline">{{ (movie || {}).Title ? movie.Title : "Not Available"}}</span>
            <div class="grey--text font-weight-light">
              {{ (movie || {}).Genre ? movie.Genre : "Not Available"}}
            </div>
            <div class="grey--text font-weight-light text-subtitle-2">
              {{ (movie || {}).Actors ? movie.Actors : "Not Available"}}
            </div>
          </div>
          <v-spacer></v-spacer>
          </v-card-title>
          <v-divider></v-divider>
          <v-card-actions>
            <span class="pl-2 grey--text text--darken-2 font-weight-light text-body-2">
              <template v-if="getRatingsData">{{ getRatingsData.Value }} ({{ getRatingsData.Source}})</template>
              <template v-else>{{ "Not Available" }}</template>
            </span>
            <v-spacer></v-spacer>
          </v-card-actions>
          <div class="pa-4 pt-0 text-body-1">
            <em>{{ (movie || {}).Plot || movie.Plot || "......." }}</em>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>

<script>
  export default {
    props: ['movieData'],
    data: () => ({
      loading: false,
      selection: 1,
      movie: {},
      offlineImage: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASwAAAGQCAYAAAAUdV17AAAXfElEQVR4Xu3d547rVrNFUTnnnHO2Yfj938J/nXPOOeeLanw8oPeVRErdbXt1DQKGgdMSVXuu0uRm1GXPP//8XxsLAgggEEDgMsIKSEmJCCBwQoCwNAICCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgLD2AAAIxBAgrJiqFIoAAYekBBBCIIUBYMVEpFAEECEsPIIBADAHCiolKoQggQFh6AAEEYggQVkxUCkUAAcLSAwggEEOAsGKiUigCCBCWHkAAgRgChBUTlUIRQICw9AACCMQQIKyYqBSKAAKEpQcQQCCGAGHFRKVQBBAgrKEHrr322s0999yzufnmmzdXXXXV5rLLLjt5xZ9//rn5/fffN99+++3mww8/3Pz2228Hd8/tt9++ufvuuzf1GVdcccWl9//xxx+bH374YfPZZ59tvv7664PXe8MNN5yst2qu9c5r/vXXXzdffPHF5tNPPz0ZQ6eluDz++OObq6+++mTY33333ebVV189CEH1wh133LG55pprNpdffvml91Yv1Poqs/r/oct59cKhdaS9nrBmiT300EMnzTmXybZASzAlgffee29V3iW+Rx555EQok0y2vfGvv/46EeI777yzWoj333//iayWav7ll18277///lFCXDXI/+CLnnrqqRPm03KIsEp21Q/1/31LZTb1wpoNwnn2wn8wgjMvibD+h/TRRx/d1FZv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",
    }),
    methods: {
      reserve () {
        this.loading = true

        setTimeout(() => (this.loading = false), 2000)
      },
    },
    computed: {
      getRatingsData: function () {
        if (!this.movie.Ratings) {
          return null;
        }

        return this.movie.Ratings[0];
      }
    },
    watch: {
      movieData(newValue) {
        this.loading = true;
        this.movie = newValue;
        setTimeout(() => (this.loading = false), 1000)
      }
    }
  }
</script>