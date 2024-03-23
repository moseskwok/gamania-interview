<script setup>
import { ref } from 'vue'
import Countdown from './components/Countdown.vue'
import InputBox from './components/InputBox.vue'
import Button from "@/components/Button.vue"

const totalTime = ref(0);
const hour = ref(0);
const min = ref(0);
const sec = ref(0);

function setTime(h, m, s){
  hour.value = parseInt(h);
  min.value = parseInt(m);
  sec.value = parseInt(s);
  totalTime.value = h*3600 + m*60 + s;
}

function resetTimer(){
  setTime(0,0,0);
}
</script>
<template>
  <div v-if="totalTime > 0" style="margin-top: 20px;">
    <Button text="返回" @click="resetTimer()"/>
    <Countdown :hour="hour" :min="min" :sec="sec"/>
  </div>
  <div v-else>
    <div><h1>倒數計時器</h1></div>
    <InputBox @set-time="setTime"/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
