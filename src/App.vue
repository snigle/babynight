<script setup lang="ts">
import {Suspense} from "vue";
import Player from "./components/Player.vue"
</script>
<script lang="ts">
// import { YoutubeVue3 } from "youtube-vue3";

// export default {
//   setup() {
    
//   },
//   data() {
//     navigator.mediaDevices.getUserMedia({ audio: true }).then((stream) => {
//       this.stream = stream;
//     });
//     return {
//       interval:0,
//       stream:null,
//       analizer: this.analizer,
//       titleClass: "title",
//       active: false,
//       config: "CGgr1nGNvLo",
//       url: "CGgr1nGNvLo",
//       firstPlayed: false,
//       lastPlayed: null,
//     };
//   },
//   components: {
//     YoutubeVue3,
//   },
//   computed() {
//     return {
//       videoId: this.url,
//     };
//   },
//   beforeUnmount() {
//     this.stop(); 
//   },
//   methods: {
//     applyConfig() {
//       this.config = this.url;
//       this.active = true;
//       // this.$refs.youtube.player.playVideo();

//       const audiocontext = new AudioContext();  
//       this.analizer = audiocontext.createAnalyser();
//       this.analizer.fftSize = 2048;
//       const microphone = audiocontext.createMediaStreamSource(this.stream);
//       microphone.connect(this.analizer);
//       // analizer.connect(audiocontext.destination)
//       const array = new Float32Array(this.analizer.fftSize);
//       this.interval = setInterval(() => {
//         this.analizer.getFloatTimeDomainData(array)
//         let sumSquares = 0.0
//         for (const amplitude of array) { sumSquares += amplitude*amplitude; }
//         const volume = Math.sqrt(sumSquares / array.length);
//         // console.log(volume)
//         if (volume > 0.05 ) {
//           console.log("play video", this.$refs.youtube)
//           this.playCurrentVideo()
//         }
//       },200)
//     },
//     stop() {
//       this.active = false;
//       clearInterval(this.interval);
//       this.interval = 0;
//       this.pauseCurrentVideo();
//     },
//     playCurrentVideo() {
//       if (!this.lastPlayed) {
//         this.lastPlayed = new Date();
//         setTimeout(() => {
//           this.pauseCurrentVideo()
//         }, 15 * 60 * 1000)
//       }  
//       // this.$refs.youtube.player.playVideo();
//     },
//     stopCurrentVideo() {
//       this.$refs.youtube.player.stopVideo();
//     },
//     pauseCurrentVideo() {
//       this.$refs.youtube.player.pauseVideo();
//     },
//     onEnded() {
//       console.log("## OnEnded");
//     },
//     onPaused() {
//       console.log("## OnPaused");
//       this.lastPlayed = null;
//     },
//     onPlayed() {
//       console.log("## OnPlayed");
//       // if (!this.firstPlayed) {
//       //   this.firstPlayed = true;
//         // this.stopCurrentVideo();
//       // }
//     },
//   },
// };
</script>

<template>
  <div>
    <header>
      <img
        alt="Vue logo"
        class="logo"
        src="./assets/logo.svg"
        width="125"
        height="125"
      />
    </header>

    <main>
      <Suspense>
        <Player />
      </Suspense>
    </main>
  </div>
</template>

<style scoped>
</style>
