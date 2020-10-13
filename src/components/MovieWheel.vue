<template>
  <v-col md="6">
    <!-- type: canvas -->
    <FortuneWheel
      style="width: 100%"
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
</template>

<script>
import FortuneWheel from "vue-fortune-wheel";
import "vue-fortune-wheel/lib/vue-fortune-wheel.css";
import movies from "../data/movies.json"

export default {
  components: {
    FortuneWheel,
  },
  data() {
    return {
      canvasVerify: true, // Whether the turntable in canvas mode is enabled for verification
      prizes: [
        {
          id: 1, //* The unique id of each prize, an integer greater than 0
          name: "Blue", // Prize name, display value when type is canvas (this parameter is not needed when type is image)
          value: "Blue's value", //* Prize value, return value after spinning
          bgColor: "#45ace9", // Background color (no need for this parameter when type is image)
          color: "#ffffff", // Font color (this parameter is not required when type is image)
          probability: 30, //* Probability, up to 4 decimal places (the sum of the probabilities of all prizes
        },
        {
          id: 2,
          name: "Red",
          value: "Red's value",
          bgColor: "#dd3832",
          color: "#ffffff",
          probability: 40,
        },
        {
          id: 3,
          name: "Yellow",
          value: "Yellow's value",
          bgColor: "#fef151",
          color: "#ffffff",
          probability: 30,
        },
      ],
    };
  },
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
        const verified = true; // true: the test passed the verification, false: the test failed the verification
        this.DoServiceVerify(verified, 2000).then((verifiedRes) => {
          if (verifiedRes) {
            console.log("Verification passed, start to rotate");
            rotate(); // Call the callback, start spinning
            this.canvasVerify = false; // Turn off verification mode
          } else {
            alert("Failed verification");
          }
        });
        return;
      }
      console.log("onCanvasRotateStart");
    },
    onRotateEnd(prize) {
      alert(prize.value);
    },
    // Simulate the request back-end interface, verified: whether to pass the verification, duration: delay time
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
    }
  },
};
</script>

<style>

</style>
