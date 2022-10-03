<template>
  <div class="city-view-skeleton">
    <!-- Overview -->
    <div class="flex flex-col py-12 items-center">
      <AnimatedPlaceholder class="max-w-[300px] w-full mb-2" />
      <AnimatedPlaceholder class="max-w-[300px] w-full mb-12" />
      <AnimatedPlaceholder
        class="max-w-[300px] h-[100px] w-full mb-12"
      />
      <AnimatedPlaceholder class="max-w-[300px] w-full mb-8" />
      <AnimatedPlaceholder
        class="max-w-[300px] h-[75px] w-full"
      />
    </div>
    <div>
      <!-- Hourly -->
      <div class="flex flex-col py-12 px-8 items-center">
        <AnimatedPlaceholder
            class="max-w-screen-md h-[100px] w-full mb-12 "
        />
      </div>
      <!-- Weekly -->
      <div class="flex flex-col py-12 px-8 items-center">
        <AnimatedPlaceholder
            class="max-w-screen-md h-[100px] w-full mb-12"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>

<style lang="scss">
  .city-view-skeleton{
    display: grid;
    grid-template-columns: 30% 67%;
    gap: 40px;
    padding-bottom: 40px;

    @media (max-width: 768px) {
      display: flex;
      flex-direction: column;
    }
  }
</style>
