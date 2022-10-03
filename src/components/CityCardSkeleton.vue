<template>
  <div
    class="flex py-6 px-3  bg-primary-green rounded-md shadow-md card-skeleton"
  >
    <div class="flex flex-col flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[50%]" />
      <AnimatedPlaceholder class="max-w-[40%]" />
    </div>

    <div class="flex flex-col items-end flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[50px] w-full" />
      <AnimatedPlaceholder class="max-w-[75px] w-full" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>

<style lang="scss">
  .card-skeleton{
    box-shadow: 0px 4px 10px rgb(0 0 0 / 20%);
    backdrop-filter: blur(35px);
  }
</style>