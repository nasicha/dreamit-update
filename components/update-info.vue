<template>
  <div class="wrapper">
    <span class="circle-percent">{{ percent }}</span>
    <span class="circle-word">Updating...</span>
    <img src="~assets/img/black-circle.png" class="circle-black" />
    <div class="reveal"></div>
    <img src="~assets/img/fancy-circle.png" class="circle-fancy" />
  </div>
</template>
<script setup lang="ts">
const percent = inject("percent");
const startUpdateAfterSec = inject("startUpdate");
const useRandomNumber = inject("useRandomNumber");
const startUpdate = ref(false);
const currentTime = ref(startUpdateAfterSec.value+.5);

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
    }, useRandomNumber.value ? randomIntFromInterval(14, 21) : 300)
  } 
}

const randomIntFromInterval = (min: number, max: number) => {
  let result = Math.pow(Math.floor(Math.random() * (max - min + 1) + min), 2);
  return result;
}
</script>
<style lang="scss">
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
    top: 50%;
    left: 0;
    right: 0;
    margin: auto;
    transform: translateY(-50%);
  }

  &-black {
    width: $size-black;
    height: $size-black;
    z-index: 10;
  }

  &-fancy {
    width: $size-fancy;
    height: $size-fancy;
    z-index: 8;
  }
}
</style>