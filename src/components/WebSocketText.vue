<template>
  <div>
    <input type="text" v-model="msg">
  <button @click="sendMsg">点击发送数据</button>
    <div v-for="(item, index) in list" :key="index" :style="{color: item.type}">{{ item.str }}</div>
  </div>
</template>

<script setup>
import {ref} from 'vue'
const msg = ref('')
const list = ref([])
const ws = new WebSocket('ws://localhost:8010')
    ws.onopen = (e) => {
      console.log('连接服务器成功', e);
      // list.value.push(str.data)
    }
    
    ws.onmessage = (str) => {
      list.value.push(JSON.parse(str.data))
    }
    ws.close = (e) => {
      console.log('close', e);
    }
    const sendMsg = (e) => {
      ws.send(msg.value)
      msg.value = ''
    }
</script>

<style lang="scss" scoped>

</style>