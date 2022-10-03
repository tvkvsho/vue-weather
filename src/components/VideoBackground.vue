<template>
  <div class="video-background">
    <div class="video-background__black-box"></div>
    <video class="video-background__video" autoplay loop muted>
      <source src="../assets/video/videoplayback.webm" type="video/webm">
      <source src="../assets/video/videoplayback.mp4" type="video/mp4">
    </video>
  </div>
</template>

<script>
export default {
  name: "VideoBackground"
}
</script>

<style lang="scss">
  .video-background{
    display: block;
    position: fixed;
    z-index: -1;

    &__black-box{
      display: block;
      background: black;
      opacity: 20%;
      height: 100vh;
      position: fixed;
      width: 100%;
      z-index: -2;
    }

    &__video{
      position: absolute;
      height: 100vh;
      max-width: fit-content;
      z-index: -3;
      transform: scale(1.3);
    }
  }
</style>