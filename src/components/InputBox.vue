<script setup>
import { ref, defineEmits } from 'vue'
import Swal from "sweetalert2";
import Button from "@/components/Button.vue";

const emits = defineEmits(['setTime'])

const day = ref('0');
const hour = ref('00');
const min = ref('00');
const sec = ref('00');

function alertMessage(text, success = 0){
  Swal.fire({
    toast: true,
    position: 'top-end',
    showConfirmButton: false,
    timer: 3000,
    timerProgressBar: true,
    icon: success ? 'success' :'error',
    title: text,
    didOpen: (toast) => {
      toast.addEventListener('mouseenter', Swal.stopTimer)
      toast.addEventListener('mouseleave', Swal.resumeTimer)
    }
  });
}

function submitForm(){
  let total = parseInt(day.value)*86400 + parseInt(hour.value)*3600 + parseInt(min.value)*60 + parseInt(sec.value);

  if(total < 1){
    alertMessage('時間不允許少於1秒');
    return false;
  }
  if(total > 345600){
    alertMessage('時間不允許超過4天');
    return false;
  }

  let finalHour = parseInt(day.value)*24 + parseInt(hour.value);
  emits('setTime',finalHour,min.value,sec.value)
  alertMessage('成功啟動倒數',1);
}
</script>

<template>
  <div>
    <form v-on:submit.prevent="submitForm">
      <div class="row">
        <span>
          <input v-model="day" class="slide-up" id="day" type="number" placeholder="X" min="0" max="4" onclick="this.select()" /><label for="day">日</label>
        </span>
        <span>
          <input v-model="hour" class="slide-up" id="hour" type="number" placeholder="XX" min="00" max="23" onclick="this.select()" /><label for="hour">時</label>
        </span>
        <span>
          <input v-model="min" class="slide-up" id="min" type="number" placeholder="XX" min="00" max="59" onclick="this.select()" /><label for="min">分</label>
        </span>
        <span>
          <input v-model="sec" class="slide-up" id="sec" type="number" placeholder="XX" min="00" max="59" onclick="this.select()" /><label for="sec">秒</label>
        </span>
        <div>
          <Button text="確認"/>
        </div>
      </div>
    </form>
  </div>
</template>

<style scoped>
@import url(https://fonts.googleapis.com/css?family=Open+Sans:400,700,600,300,800);
* {
  box-sizing: border-box;
  color: #ffffff
}

.row {
  margin: 0 auto;
  padding: 60px 30px;
  background: #fffb00;
  position: relative;
  z-index: 1;
  text-align: center;
}

.row span {
  position: relative;
  display: inline-block;
  margin: 30px 10px;
}

.slide-up {
  display: inline-block;
  width: 125px;
  padding: 10px 0 10px 15px;
  font-family: "Open Sans", sans;
  font-weight: 400;
  color: #377D6A;
  background: #efefef;
  border: 0;
  border-radius: 3px;
  outline: 0;
  text-indent: 50px;
  transition: all .3s ease-in-out;
}
.slide-up::-webkit-input-placeholder {
  color: #efefef;
  text-indent: 0;
  font-weight: 300;
}
.slide-up + label {
  display: inline-block;
  position: absolute;
  transform: translateX(0);
  top: 0;
  left: 0;
  padding: 8px 15px;
  text-shadow: 0 1px 0 rgba(19, 74, 70, 0.4);
  transition: all .3s ease-in-out;
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
  overflow: hidden;
}
.slide-up + label:before, .slide-up + label:after {
  content: "";
  position: absolute;
  right: 0;
  left: 0;
  z-index: -1;
  transition: all .3s ease-in-out;
}
.slide-up + label:before {
  top: 6px;
  left: 5px;
  right: 5px;
  bottom: 6px;
  background: hsl(0, 100%, 73%);
}
.slide-up + label:after {
  top: 0;
  bottom: 0;
  background: hsl(0, 100%, 73%);
}

.slide-up:focus,
.slide-up:active {
  color: #377D6A;
  text-indent: 0;
  background: #fff;
}
.slide-up:focus::-webkit-input-placeholder,
.slide-up:active::-webkit-input-placeholder {
  color: #aaa;
}
.slide-up:focus + label,
.slide-up:active + label {
  transform: translateY(-100%);
}
.slide-up:focus + label:before,
.slide-up:active + label:before {
  border-radius: 5px;
}
.slide-up:focus + label:after,
.slide-up:active + label:after {
  transform: translateY(100%);
}

</style>