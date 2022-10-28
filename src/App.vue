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
      <nav class="navbar navbar-expand-lg bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Dropdown
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled">Disabled</a>
        </li>
      </ul>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
    </header>

    <div>
      <Suspense>
        <Player />
      </Suspense>
    </div>
  </div>
</template>

<style scoped>
</style>
