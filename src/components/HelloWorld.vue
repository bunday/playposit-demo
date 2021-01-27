<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <p class="subheading font-weight-regular">Please click on the setup button twice to start this Demo</p>
      </v-col>
      <v-col class="mb-5" cols="12">
        <video
          width="980"
          height="640"
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
          <v-btn
            @click="setUpControlListener()"
            color="blue"
            class="ma-2 white--text"
          >
            <v-icon left blue> mdi-cog </v-icon>
            Setup
          </v-btn>
          <v-btn @click="play()" color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-play </v-icon>
            Play
          </v-btn>
          <v-btn @click="pause()" color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-pause </v-icon>
            Pause
          </v-btn>
          <v-btn @click="stepForward()" color="blue" class="ma-2 white--text">
            <v-icon left blue> mdi-fast-forward </v-icon>
            Forward
          </v-btn>
        </v-row>
      </v-col>
      <v-col class="mb-5" cols="12">
        <h2 class="headline font-weight-bold mb-3">Camera Status</h2>

        <v-form>
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="camera.position.x"
                label="X Coordinate"
                required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                v-model="camera.position.y"
                label="Y Coordinate"
                required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                v-model="camera.position.z"
                label="Z Coordinate"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-form>
        <v-row justify="center">
          <v-card elevation="2" class="ma-2">
            <v-card-title>
              {{
                camera ? (camera.position ? camera.position.x : "N/A") : "N/A"
              }}</v-card-title
            >
            <v-card-text>X Coordinates</v-card-text>
          </v-card>

          <v-card elevation="2" class="ma-2">
            <v-card-title>
              {{
                camera ? (camera.position ? camera.position.y : "N/A") : "N/A"
              }}</v-card-title
            >
            <v-card-text>Y Coordinates</v-card-text>
          </v-card>
          <v-card elevation="2" class="ma-2">
            <v-card-title>
              {{
                camera ? (camera.position ? camera.position.z : "N/A") : "N/A"
              }}</v-card-title
            >
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
import { CSS2DRenderer, CSS2DObject } from "../plugins/CSS2DRenderer";

export default {
  name: "HelloWorld",
  mounted() {
    this.setUpVideoVR();
  },
  methods: {
    setUpVideoVR() {
      // get the ref for the video
      this.player = videojs(this.$refs.interactiveVideo);
      // set up the VR + 360 projection
      this.player.vr({ debug: true, projection: "360" });
    },
    setUpControlListener() {
      const cameraStatus = this.player.vr().camera;
      this.camera = { ...cameraStatus };
      // create a control object and attach a positio change listener to it
      const controls = new OrbitControls(
        this.player.vr().camera,
        this.player.vr().renderer.domElement
      );
      controls.addEventListener("change", this.updateCameraPosition);
      this.addElement();
    },
    getPlayer() {
      // get the camera status and update the camera data
      const cameraStatus = this.player.vr().camera;
      this.camera = { ...cameraStatus };
    },
    updateCameraPosition() {
      this.getPlayer();
    },
    play() {
      if (this.player.paused()) {
        // if paused, play it
        this.player.play();
      }
    },
    pause() {
      if (!this.player.paused()) {
        // if not paused, pause it
        this.player.pause();
      }
    },
    stepForward() {
      const newTime = this.player.currentTime() + 10; // get the current time and go forward by 10sec
      this.player.currentTime(newTime); // set that as the new time
    },
    addElement() {
      let { scene, camera } = this.player.vr();
      const link = this.addInformationLink();
      scene.add(link.object);
      camera.position.z = 5;
      link.labelRenderer.render(scene, camera);
    },
    addInformationLink() {
      var element = document.createElement("a");
      element.className = "hotspot";
      element.innerHTML = "Information about the park";
      element.target = '_blank';
      element.href = 'https://en.wikipedia.org/wiki/Parks_and_open_spaces_in_London';
      var object = new CSS2DObject(element);

      object.position.x = 0;
      object.position.y = 0;
      object.position.z = 7;

      let labelRenderer = new CSS2DRenderer();
      labelRenderer.setSize(400, 400);
      labelRenderer.domElement.style.position = "absolute";
      labelRenderer.domElement.style.top = "300px";
      labelRenderer.domElement.style.left = "780px";
      document.body.appendChild(labelRenderer.domElement);
      return { object, labelRenderer}
    },
    addIframeVideo() {
      var element = document.createElement("iframe");
      element.className = "hotspot-iframe";
      element.height="200" 
      element.width="300" 
      element.title="Iframe Duck video"
      
      element.src = 'https://www.youtube.com/embed/KESOibZQlhA';
      var object = new CSS2DObject(element);

      object.position.x = 0;
      object.position.y = 0;
      object.position.z = 7;

      let labelRenderer = new CSS2DRenderer();
      labelRenderer.setSize(400, 400);
      labelRenderer.domElement.style.position = "absolute";
      labelRenderer.domElement.style.top = "100px";
      labelRenderer.domElement.style.left = "480px";
      document.body.appendChild(labelRenderer.domElement);
      return { object, labelRenderer}
    }
  },
  data: () => ({
    player: null,
    camera: { position: { x: "", y: "", z: "" } },
  }),
};
</script>
<style>
@import url("https://vjs.zencdn.net/7.2.3/video-js.css");
.hotspot {
  width: 200px;
  background: #e4bdbe;
  border-radius: 10%;
  padding: 5px;
  cursor: pointer;
  text-decoration: none;
}
.hotspot:hover {
  background: #e44db1;
}
.video-js .vjs-tech {
  position: relative !important;
}
</style>
