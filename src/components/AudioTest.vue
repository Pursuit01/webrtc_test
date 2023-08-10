<template>
  <div>
    <audio ref="video" autoplay controls>播放捕获的声音</audio>
    <button @click="onOpenAudio">打开麦克风</button>
  </div>
</template>

<script setup>
import { ref } from "vue";
const video = ref();

const constrains = {
  video: false,
  audio: true,
};
const onOpenAudio = (e) => {
  navigator.mediaDevices
    .getUserMedia(constrains)
    .then((stream) => {
      video.value.srcObject = stream;
    })
    .catch((err) => {
      console.log(err);
    });
};
</script>


<style lang="scss" scoped></style>
