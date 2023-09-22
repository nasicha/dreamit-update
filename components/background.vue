<template>
  <div class="video-wrapper">
    <video src="~assets/video/bgWaves.mp4" autoplay muted>
        <p>Your browser does not support the video tag.</p>
    </video>
  </div>
</template>
<style lang="scss">
.video-wrapper {
  position: absolute;
  width: 100vw;
  height: 100vh;
  z-index: -1;

  video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}
</style>