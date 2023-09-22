<template>
  <div class="page">
    <Background />
    <div class="top">
      <SmartphoneTopbar />
    </div>
    <div class="mid">
      <UpdateInfo />
      <CancelUpdate />
    </div>
    <div class="bottom"></div>
  </div>
</template>
<script setup lang="ts">
import SmartphoneTopbar from "@/assets/img/smartphone-topbar.svg?component";
import CancelUpdate from "@/assets/img/cancel-update.svg?component";
</script>
<style lang="scss">
.page {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.top {
  height: 5vh;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  z-index: 20;
  position: relative;

  svg {
    padding: 1rem;
  }
}

.mid {
  height: 83vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}

.bottom {
  height: 12vh;
}

.word {
  letter-spacing: 0.5px;
}
</style>
