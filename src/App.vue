<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import VideoTest from "./components/VideoTest.vue";
import AudioTest from "./components/AudioTest.vue";
import WebSocketText from "./components/WebSocketText.vue";
import Download from "./components/Download.vue";
</script>

<template>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div> -->
  <Download></Download>
  <!-- <HelloWorld /> -->
  <!-- <VideoTest></VideoTest> -->
  <!-- <AudioTest></AudioTest> -->
  <!-- <WebSocketText></WebSocketText> -->
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
