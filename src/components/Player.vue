<script setup lang="ts">
import { computed, ref } from '@vue/reactivity';
import { onMounted, onUnmounted } from '@vue/runtime-core';
import { YoutubeVue3 } from "youtube-vue3";

const micAnalyser = await getMicAnalyser()

const playerNode = ref<typeof YoutubeVue3 | null>(null);
const url = ref("https://www.youtube.com/watch?v=CGgr1nGNvLo")
const videoId = computed(() : string => {
    const matches = url.value.match(/https:\/\/www.youtube.com\/watch\?v=([\w\-]+)/)
    return matches && matches.length ? matches[1] : "";
});
const listening = ref(false)
const interval = ref(0)
const timeout = ref(0) 
const volume = ref(0)

onUnmounted(() => {
    stopListeningMic()
})

// DOM methods
function startListeningMic() {
  if (!playerNode.value) {
    return;
  }
  const player = playerNode.value.player;
  listening.value = true
  interval.value = setInterval(() => {
        volume.value = getVolume(micAnalyser)
        if (volume.value > 0.04 ) {
            console.log(volume)
            playDuringPeriod(player, 15 * 60 * 1000)
        }
      },200)
}

function stopListeningMic() {
  if (!playerNode.value) {
    return;
  }
  listening.value = false;
  clearInterval(interval.value);
  clearTimeout(timeout.value);
  playerNode.value.player.pauseVideo();
}


function playDuringPeriod(player: any, duration: number): void {
    player.playVideo()
    clearInterval(interval.value);
    timeout.value = setTimeout(() => {
        player.pauseVideo()
        startListeningMic()
    }, duration)
}


// Lib methods
async function getMicAnalyser(): Promise<AnalyserNode> {
  const micStream = await navigator.mediaDevices.getUserMedia({ audio: true });
  const audiocontext = new AudioContext();  
  const analizer = audiocontext.createAnalyser();
  analizer.fftSize = 2048;
  const microphone = audiocontext.createMediaStreamSource(micStream);
  microphone.connect(analizer);
  return analizer;
}

function getVolume(analyser: AnalyserNode) : number {
  const array = new Float32Array(analyser.fftSize);
  analyser.getFloatTimeDomainData(array)
  let sumSquares = 0.0
  for (const amplitude of array) { sumSquares += amplitude*amplitude; }
  const volume = Math.sqrt(sumSquares / array.length);
  return volume
}

</script>

<template>
      <div v-if="!listening">
      <input type="text" v-model="url" />
      <button class="btn btn-primary" @click="startListeningMic()">Activate</button>
      </div>
      <div v-else>
      <button @click="stopListeningMic()">Stop</button>
      <input :value="volume" />
      </div>
      <YoutubeVue3
        ref="playerNode"
        :videoid="videoId"
        :loop="1"
        :width="480"
        :height="320"
        :autoplay="0"
        :controls="1"
      />
</template>

<style scoped>
</style>
