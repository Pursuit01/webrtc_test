<template>
  <div>
    <video ref="video" autoplay playsinline></video>
    <button @click="onOpenCamera">点击打开摄像头</button>
  </div>
</template>

<script setup>
import { ref } from "vue";
const video = ref();

const constrains = {
  video: true,
  audio: false,
};
const onOpenCamera = (e) => {
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
