<template>
  <Transition name="fade-grow">
    <div class="wrapper">
      <span class="circle-percent">{{ percent }}</span>
      <span class="circle-word">Updating...</span>
      <img src="~assets/img/black-circle.png" class="circle-black" />
      <div :style="{animationDuration: `${animationDuration}ms`, animationDelay: `-${animationDelay}ms`}" class="circle-fancy" :class="{'circle-fancy--animate': startUpdate}" />
    </div>
  </Transition>
</template>
<script setup lang="ts">
const percent = inject("percent") as Ref<number>;
const startUpdateAfterSec = inject("startUpdate") as Ref<number>;
const startUpdate = ref(false);
const currentTime = ref(startUpdateAfterSec.value+0.5);
const percentDuration = ref(300);
const animationDuration = 100 * percentDuration.value;
const animationDelay = percent.value * percentDuration.value;

const updateFinished = ref(false);

onMounted(() => {
  const updateInterval = setInterval(() => {
    if(currentTime.value > 0) {
      currentTime.value--;
    } else {
      startUpdate.value = true;
      clearInterval(updateInterval);
    }
  }, 1000);
});

watch(startUpdate, (value) => {
  if (value) {
    increasePercentage();
  }
});

const increasePercentage = () => {
  if (percent.value < 100) {
    percent.value++;
    setTimeout(() => {
      increasePercentage();
    }, percentDuration.value)
  } else {
    updateFinished.value = true;
  }
}

</script>
<style lang="scss" scoped>
$size-wrapper: 330px;
$size-black: 260px;
$size-fancy: 290px;
$font-size-percentage: 6rem;

.wrapper {
  position: relative;
  margin-top: 100px;
  width: $size-wrapper;
  height: $size-wrapper;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}
.circle {
  &-percent, &-word {
    color: white;
    z-index: 11;
  }

  &-percent {
    position: relative;
    font-size: $font-size-percentage;
    margin-right: 25px;
    font-weight: 500;
    
    &::after {
      content: '%';
      position: absolute;
      font-size: calc($font-size-percentage * 0.5);
      bottom: 23px;
      left: 100%;
      margin-left: 4px;
    }
  }

  &-word {
    color: white;
    font-size: 1.5rem;
  }

  &-black, &-fancy {
    position: absolute;    
    left: 0;
    right: 0;
    margin: auto;
  }

  &-black {
    top: 50%;
    transform: translateY(-50%);
    width: $size-black;
    height: $size-black;
    z-index: 10;
  }

  &-fancy {
    width: $size-fancy;
    height: $size-fancy;
    background: url('./assets/img/fancy-circle.png') no-repeat center center;
    background-size: $size-fancy;
    z-index: 8;
    opacity: 1;

    &--animate {
      opacity: 1;
      animation: circle linear;
    }
  }
  @keyframes circle {
    0% {
      clip-path: polygon(50% 0,400% 0,400% 50%,50% -400%,50% 50%);
    }
    25% {
      clip-path: polygon(50% 0,400% 0,400% 50%,400% 50%,50% 50%);    
    }
    50% {    
      clip-path: polygon(50% 0,400% 0,400% 50%,50% 400%,50% 50%);
    }
    75% {
      clip-path: polygon(50% 0,400% 0%,50% 400%,-300% 50%,50% 50%);
    }
    100% { 
      clip-path: polygon(50% 0,400% 0%,-300% 400%,50% -300%,50% 50%);
    }
  }
}


</style>