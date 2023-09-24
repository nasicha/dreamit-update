<template>
    <Transition name="fade">
    <div v-if="!hideOverlay" class="overlay">
      <Transition name="fade">
        <div class="overlay-wrapper" v-if="!countdownStarted && showUpdatePage">
          <div>
            <p>Startprozente:</p>
            <input v-model="percent" type="number" />
          </div>
            <!-- <input type="range" v-model="animationDuration" min="200" max="400" step="20" class="slider" />
            <p>10% dauern <span>{{ animationDuration/100 }} Sekunden</span>; 1% - <span>{{ animationDuration }}ms.</span>
            </p> -->
          <div>
            <p>Start:</p>
            <input v-model="timer" type="number" />
          </div>
          <p>
            Update startet in <span>{{ timer }} {{ timer === 1 ? 'Sekunde' : 'Sekunden' }}.</span>
          </p>
          <div>
            <p>Wechsel:</p>
            <input v-model="startUpdateAfterSec" type="number" />
          </div>
          <p>
            Screen wechselt bei <span>{{ startUpdateAfterSec }} {{ startUpdateAfterSec === 1 ? 'Sekunde' : 'Sekunden' }}.</span>
          </p>



          <button @click="startCountdown">Start</button>
        </div>
      </Transition>
      <div class="overlay-wrapper bigTime" v-if="countdownStarted">
        <h1>{{ timer }}</h1>
      </div>
    </div>
  </Transition>
  <Transition name="fade-grow">
    <UpdatePage v-if="countdownStarted"  />
  </Transition>
</template>
<script setup lang="ts">
const percent = ref(0);
const timer = ref(0);
const startUpdateAfterSec = ref(0);
const countdownStarted = ref(false);
const hideOverlay = ref(false);
const showUpdatePage = ref(false);
const animationDuration = ref(300);

onMounted(() => {
  if (process.client) {
    const storedPercent = localStorage.getItem("percent-update");
    if (storedPercent === null) {
      percent.value = 0;
      localStorage.setItem("percent-update", percent.value.toString());
    } else {
      percent.value = parseInt(storedPercent);
    }
    const storedTimer = localStorage.getItem("timer");
    if (storedTimer === null) {
      timer.value = 5;
      localStorage.setItem("timer", timer.value.toString());
    } else {
      timer.value = parseInt(storedTimer);
    }
    const storedStartUpdate = localStorage.getItem("startUpdate");
    if (storedStartUpdate === null) {
      startUpdateAfterSec.value = 3;
      localStorage.setItem("startUpdate", startUpdateAfterSec.value.toString());
    } else {
      startUpdateAfterSec.value = parseInt(storedStartUpdate);
    }
    const storedAnimationDuration = localStorage.getItem("animationDuration");
    if (storedAnimationDuration === null) {
      animationDuration.value = 300;
      localStorage.setItem("animationDuration", animationDuration.value.toString());
    } else {
      animationDuration.value = parseInt(storedAnimationDuration);
    }
  }
  showUpdatePage.value = true;
});

const startCountdown = () => {
  countdownStarted.value = true;
  localStorage.setItem("percent-update", percent.value.toString());
  localStorage.setItem("timer", timer.value.toString());
  localStorage.setItem("startUpdate", startUpdateAfterSec.value.toString());
  localStorage.setItem("animationDuration", animationDuration.value.toString());
  const countdownInterval = setInterval(() => {
    if (timer.value > 0) {
      timer.value--;
    } else {
      clearInterval(countdownInterval);
    }
  }, 1000);
};

provide("percent", percent);
provide("startUpdate", startUpdateAfterSec);
provide("animationDuration", animationDuration);

watch(timer, (value) => {
  if (value <= startUpdateAfterSec.value && countdownStarted.value) {
    hideOverlay.value = true;
  }
});

useHead({
  title: "DreamIt - Update",
  link: [
    {
      rel: "preload",
      as: "video",
      href: "../assets/video/bgWaves.mp4",
      type: "video/mp4",
    },
  ],
});
</script>
<style lang="scss">
@use '../assets/scss/transition.scss';

.overlay {
  position: absolute;
  height: 100vh;
  width: 100vw;
  background-color: black;
  z-index: 100;
  color: white;

  &-wrapper {
    display: flex;
    flex-direction: column;
    padding: 2rem;
    height: 80vh;

    &.bigTime {
      justify-content: center;
      align-items: center;
      & h1 {
        font-size: 5rem;
      }
    }

    & div {
      display: flex;
      align-items: center;
      justify-content: flex-start;
      gap: 1rem;
    }

    & span {
      text-decoration: underline;
    }

    & button {
      height: 50px;
      background-color: black;
      color: white;
      border-color: white;
      margin-top: auto;
    }
  }
}
input[type=range] {
  margin: 24px 0 8px;
	appearance: none;
	width: 100%;
	border-radius: 12px;
	height: 12px;
	border: 1px solid ;
	background-color: white; 

  &::-webkit-slider-thumb {
      width: 20px;
      -webkit-appearance: none;
      height: 20px;
      border-radius: 100%;
      background: #efefef;
      box-shadow: 0 0 4px 6px rgba(200,200,200,0.5);
    }
}
</style>