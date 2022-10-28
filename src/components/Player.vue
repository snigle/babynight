<script setup lang="ts">
import { computed, ref } from '@vue/reactivity';
import { onMounted, onUnmounted } from '@vue/runtime-core';
import { YoutubeVue3 } from "youtube-vue3";

type SoundEvent = {
volume: number
date: Date
};

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
const history = ref<SoundEvent[]>([])
var wakeLock : any = null;
onUnmounted(() => {
    stopListeningMic()
})

// DOM methods
async function startListeningMic() {
  if (!playerNode.value) {
    return;
  }
  const player = playerNode.value.player;
  listening.value = true
  try {
    wakeLock = await (navigator as any).wakeLock.request('screen');
  } catch(e) {
    console.log("can't block lockscreen")
  }
  interval.value = setInterval(() => {
        volume.value = getVolume(micAnalyser)
        if (volume.value > 0.04 ) {
            playDuringPeriod(player, 15 * 60 * 1000)
            history.value.push({
              date: new Date(),
              volume: volume.value,
            })
        }
      },200)
}

function stopListeningMic() {
  if (!playerNode.value) {
    return;
  }
  if (wakeLock) {
    wakeLock.release().then(() => wakeLock = null);
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
  <div class="container-fluid">
    <div class="row">
      <div class="col-xs-12 col-lg-6 offset-lg-3 player">
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
          :width="-1"
          :height="320"
          :autoplay="0"
          :controls="1"
        />

        <div v-for="event,i in history" :key="i">
          {{event.date.toLocaleTimeString()}}: {{event.volume}}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.player{
  border:red 1px solid
}
</style>
