<script setup>
import { ref, computed } from 'vue'
import Countdown from './components/Countdown.vue'
import InputBox from './components/InputBox.vue'
import Button from "@/components/Button.vue"

const hour = ref(0);
const min = ref(0);
const sec = ref(0);
const totalTime = computed(() => {
  return hour.value*3600 + min.value*60 + sec.value;
});

function setTime(h, m, s){
  hour.value = parseInt(h);
  min.value = parseInt(m);
  sec.value = parseInt(s);
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
