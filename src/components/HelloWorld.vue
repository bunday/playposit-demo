<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <p class="subheading font-weight-regular">Instruction will go here</p>
      </v-col>
      <v-col class="mb-5" cols="12">
        <video
          width="640"
          height="300"
          id="videojs-vr-player"
          ref="interactiveVideo"
          class="video-js vjs-default-skin"
          controls
          playsinline
        >
          <source src="../assets/video/London_Park_Ducks_Swans.mp4" />
        </video>
      </v-col>
      <v-col class="mb-5" cols="12">
        <h2 class="headline font-weight-bold mb-3">Controller</h2>

        <v-row justify="center">
          <v-btn @click="setUpControlListener()" color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-cog </v-icon>
            Setup 
          </v-btn>
          <v-btn color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-play </v-icon>
            Play
          </v-btn>
          <v-btn color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-pause </v-icon>
            Pause
          </v-btn>
          <v-btn color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-fast-forward </v-icon>
            Forward
          </v-btn>
        </v-row>
      </v-col>
      <v-col class="mb-5" cols="12">
        <h2 class="headline font-weight-bold mb-3">Camera Status</h2>

        <v-row justify="center">
          <v-card elevation="2" class="ma-2">
            <v-card-title> N/A</v-card-title>
            <v-card-text>X Coordinates</v-card-text>
          </v-card>

          <v-card elevation="2" class="ma-2">
            <v-card-title> N/A</v-card-title>
            <v-card-text>Y Coordinates</v-card-text>
          </v-card>
          <v-card elevation="2" class="ma-2">
            <v-card-title> N/A</v-card-title>
            <v-card-text>Z Coordinates</v-card-text>
          </v-card>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import videojs from "video.js";
import "videojs-vr/dist/videojs-vr.min.js";
import { OrbitControls } from "../plugins/OrbitControls.js";

export default {
  name: "HelloWorld",
  mounted() {
    this.setUpVideoVR();
  },
  methods: {
    setUpVideoVR() {
      this.player = videojs(this.$refs.interactiveVideo);

      this.player.vr({ debug: true, projection: "360" });
    },
    setUpControlListener() {
      const cameraStatus = this.player.vr().camera;
      this.camera = { ...cameraStatus };
      const controls = new OrbitControls(
        this.player.vr().camera,
        this.player.vr().renderer.domElement
      );
      controls.addEventListener("change", this.updateCameraPosition);
    },
    getPlayer() {
      const cameraStatus = this.player.vr().camera;
      this.camera = { ...cameraStatus };
    },
    updateCameraPosition() {
      this.getPlayer();
    },
  },
  data: () => ({
    player: null,
    camera: null,
    ecosystem: [
      {
        text: "vuetify-loader",
        href: "https://github.com/vuetifyjs/vuetify-loader",
      },
      {
        text: "github",
        href: "https://github.com/vuetifyjs/vuetify",
      },
      {
        text: "awesome-vuetify",
        href: "https://github.com/vuetifyjs/awesome-vuetify",
      },
    ],
    importantLinks: [
      {
        text: "Documentation",
        href: "https://vuetifyjs.com",
      },
      {
        text: "Chat",
        href: "https://community.vuetifyjs.com",
      },
      {
        text: "Made with Vuetify",
        href: "https://madewithvuejs.com/vuetify",
      },
      {
        text: "Twitter",
        href: "https://twitter.com/vuetifyjs",
      },
      {
        text: "Articles",
        href: "https://medium.com/vuetify",
      },
    ],
    whatsNext: [
      {
        text: "Explore components",
        href: "https://vuetifyjs.com/components/api-explorer",
      },
      {
        text: "Select a layout",
        href: "https://vuetifyjs.com/getting-started/pre-made-layouts",
      },
      {
        text: "Frequently Asked Questions",
        href:
          "https://vuetifyjs.com/getting-started/frequently-asked-questions",
      },
    ],
  }),
};
</script>
<style scoped>
@import url("https://vjs.zencdn.net/7.2.3/video-js.css");
.hotspot {
  width: 50px;
  background: #e4bdbe;
  border-radius: 10%;
  padding: 5px;
  cursor: pointer;
}
.hotspot:hover {
  background: #e44db1;
}
</style>
