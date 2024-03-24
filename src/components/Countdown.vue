<script setup>
import {onMounted, onUnmounted, defineProps, ref} from 'vue'
import Swal from "sweetalert2";
import Button from "@/components/Button.vue";

const props = defineProps(['hour','min','sec']);
const isStarted = ref(false);
const isCounting = ref(false);
const hour = ref(0);
const min = ref(0);
const sec = ref(0);
const targetDate = ref(new Date());

function getTimeSegmentElements(segmentElement) {
  const segmentDisplay = segmentElement.querySelector('.segment-display');
  const segmentDisplayTop = segmentDisplay.querySelector('.segment-display__top');
  const segmentDisplayBottom = segmentDisplay.querySelector('.segment-display__bottom');

  const segmentOverlay = segmentDisplay.querySelector('.segment-overlay');
  const segmentOverlayTop = segmentOverlay.querySelector('.segment-overlay__top');
  const segmentOverlayBottom = segmentOverlay.querySelector('.segment-overlay__bottom');

  return {
    segmentDisplayTop,
    segmentDisplayBottom,
    segmentOverlay,
    segmentOverlayTop,
    segmentOverlayBottom,
  };
}

function updateSegmentValues(
    displayElement,
    overlayElement,
    value
) {
  displayElement.textContent = value;
  overlayElement.textContent = value;
}

function updateTimeSegment(segmentElement, timeValue) {
  const segmentElements = getTimeSegmentElements(segmentElement);

  if (parseInt(segmentElements.segmentDisplayTop.textContent, 10) === timeValue) {
    return;
  }

  segmentElements.segmentOverlay.classList.add('flip');

  updateSegmentValues(
    segmentElements.segmentDisplayTop,
    segmentElements.segmentOverlayBottom,
    timeValue
  );

  function finishAnimation() {
    segmentElements.segmentOverlay.classList.remove('flip');
    updateSegmentValues(
      segmentElements.segmentDisplayBottom,
      segmentElements.segmentOverlayTop,
      timeValue
    );

    this.removeEventListener(
      'animationend',
      finishAnimation
    );
  }

  segmentElements.segmentOverlay.addEventListener(
    'animationend',
    finishAnimation
  );
}

function updateTimeSection(sectionID, timeValue) {
  const firstNumber = Math.floor(timeValue / 10) || 0;
  const secondNumber = timeValue % 10 || 0;
  const sectionElement = document.getElementById(sectionID);
  const timeSegments = sectionElement.querySelectorAll('.time-segment');

  updateTimeSegment(timeSegments[0], firstNumber);
  updateTimeSegment(timeSegments[1], secondNumber);
}

function getTimeRemaining(targetDateTime) {
  const nowTime = Date.now();
  const complete = nowTime >= targetDateTime;

  if (complete) {
    return {
      complete,
      seconds: 0,
      minutes: 0,
      hours: 0,
    };
  }

  const secondsRemaining = Math.floor((targetDateTime - nowTime) / 1000);
  const hours = Math.floor(secondsRemaining / 60 / 60);
  const minutes = Math.floor(secondsRemaining / 60) - hours * 60;
  const seconds = secondsRemaining % 60;

  return {
    complete,
    seconds,
    minutes,
    hours,
  };
}

function updateAllSegments() {
  const timeRemainingBits = getTimeRemaining(new Date(targetDate.value).getTime());

  updateTimeSection('seconds', timeRemainingBits.seconds);
  updateTimeSection('minutes', timeRemainingBits.minutes);
  updateTimeSection('hours', timeRemainingBits.hours);

  return timeRemainingBits.complete;
}

function startTimer(){
  isStarted.value = true;
  isCounting.value = true;
  targetDate.value = new Date();
  targetDate.value.setHours(targetDate.value.getHours() + hour.value);
  targetDate.value.setMinutes(targetDate.value.getMinutes() + min.value);
  targetDate.value.setSeconds(targetDate.value.getSeconds() + sec.value + 1);

  countdownTimer = setInterval(() => {
    const isComplete = updateAllSegments();

    if (isComplete) {
      stopTimer();
      Swal.fire({
        title: '倒數已結束！',
      });
    }
  }, 1000);
}
function stopTimer(){
  const timeRemainingBits = getTimeRemaining(new Date(targetDate.value).getTime());
  hour.value = timeRemainingBits.hours;
  min.value = timeRemainingBits.minutes;
  sec.value = timeRemainingBits.seconds;

  isCounting.value = false;
  clearInterval(countdownTimer);
}

let countdownTimer = null;

onMounted(() => {
  hour.value = props.hour;
  min.value = props.min;
  sec.value = props.sec;

  updateTimeSection('hours', hour.value);
  updateTimeSection('minutes', min.value);
  updateTimeSection('seconds', sec.value);
});

onUnmounted(() => {
  if(isCounting.value){
    stopTimer();
  }
});

</script>

<template>
  <div>
    <div class="badge-div">
      <div v-show="isStarted && !isCounting" class="badge">
        Pause ⏸️
      </div>
    </div>

    <div class="countdown">
      <div class="time-section" id="hours">
        <div class="time-group">
          <div class="time-segment">
            <div class="segment-display">
              <div class="segment-display__top"></div>
              <div class="segment-display__bottom"></div>
              <div class="segment-overlay">
                <div class="segment-overlay__top"></div>
                <div class="segment-overlay__bottom">       </div>
              </div>
            </div>
          </div>
          <div class="time-segment">
            <div class="segment-display">
              <div class="segment-display__top"></div>
              <div class="segment-display__bottom"></div>
              <div class="segment-overlay">
                <div class="segment-overlay__top"></div>
                <div class="segment-overlay__bottom"></div>
              </div>
            </div>
          </div>
        </div>
        <p>時</p>
      </div>

      <div class="time-section" id="minutes">
        <div class="time-group">
          <div class="time-segment">
            <div class="segment-display">
              <div class="segment-display__top"></div>
              <div class="segment-display__bottom"></div>
              <div class="segment-overlay">
                <div class="segment-overlay__top"></div>
                <div class="segment-overlay__bottom"></div>
              </div>
            </div>
          </div>
          <div class="time-segment">
            <div class="segment-display">
              <div class="segment-display__top"></div>
              <div class="segment-display__bottom"></div>
              <div class="segment-overlay">
                <div class="segment-overlay__top"></div>
                <div class="segment-overlay__bottom"></div>
              </div>
            </div>
          </div>
        </div>
        <p>分</p>
      </div>

      <div class="time-section" id="seconds">
        <div class="time-group">
          <div class="time-segment">
            <div class="segment-display">
              <div class="segment-display__top"></div>
              <div class="segment-display__bottom"></div>
              <div class="segment-overlay">
                <div class="segment-overlay__top"></div>
                <div class="segment-overlay__bottom"></div>
              </div>
            </div>
          </div>
          <div class="time-segment">
            <div class="segment-display">
              <div class="segment-display__top"></div>
              <div class="segment-display__bottom"></div>
              <div class="segment-overlay">
                <div class="segment-overlay__top"></div>
                <div class="segment-overlay__bottom"></div>
              </div>
            </div>
          </div>
        </div>
        <p>秒</p>
      </div>
    </div>

    <div>
      <Button v-if="isCounting" text="暫停" @click="stopTimer()"/>
      <Button v-else :text="isStarted ? '繼續' : '開始'" @click="startTimer()"/>
    </div>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.badge-div{
  min-height: 30px;
}

.badge {
  background-color: red;
  color: white;
  padding: 4px 8px;
  text-align: center;
  border-radius: 5px;
  margin: 0 auto;
  width: 100px;
  height: 30px;
}

.countdown {
  padding: 50px 0 10px 0;
  width: 100%;
  display: flex;
  gap: 30px;
  font-family: sans-serif;
  justify-content: center;
  background: #fffb00;
  margin: 20px 0 20px 0;
}

@media screen and (max-width:800px) {
  .countdown {
    display: grid;
  }
}

  .time-section {
  text-align: center;
  font-size: 30px;
}

.time-group {
  display: flex;
  gap: 10px;
}

.time-segment {
  display: block;
  font-size: 100px;
  font-weight: 900;
  width: 100px;
}

.segment-display {
  position: relative;
  height: 100%;
}

.segment-display__top,
.segment-display__bottom {
  overflow: hidden;
  text-align: center;
  width: 100%;
  height: 50%;
  position: relative;
}

.segment-display__top {
  line-height: 1.5;
  color: #eee;
  background-color: #111;
}

.segment-display__bottom {
  line-height: 0;
  color: #fff;
  background-color: #333;
}

.segment-overlay {
  position: absolute;
  top: 0;
  perspective: 400px;
  height: 100%;
  width: 100px;
}

.segment-overlay__top,
.segment-overlay__bottom {
  position: absolute;
  overflow: hidden;
  text-align: center;
  width: 100%;
  height: 50%;
}

.segment-overlay__top {
  top: 0;
  line-height: 1.5;
  color: #fff;
  background-color: #111;
  transform-origin: bottom;
}

.segment-overlay__bottom {
  bottom: 0;
  line-height: 0;
  color: #eee;
  background-color: #333;
  border-top: 2px solid black;
  transform-origin: top;
}

.segment-overlay.flip .segment-overlay__top {
  animation: flip-top 0.8s linear;
}

.segment-overlay.flip .segment-overlay__bottom {
  animation: flip-bottom 0.8s linear;
}

@keyframes flip-top {
  0% {
    transform: rotateX(0deg);
  }
  50%,
  100% {
    transform: rotateX(-90deg);
  }
}

@keyframes flip-bottom {
  0%,
  50% {
    transform: rotateX(90deg);
  }
  100% {
    transform: rotateX(0deg);
  }
}

</style>