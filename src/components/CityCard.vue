<template>
  <div
    class="flex py-6 px-6 rounded-md shadow-md cursor-pointer city-card"
  >
    <div class="flex flex-col flex-1 city-card__titles">
      <h2 class="text-3xl city-card__h2">{{ city.city }}</h2>
      <h3 class="city-card__h3">{{ city.state }}</h3>
    </div>

    <div class="flex flex-col gap-2 city-card__box">
      <p class="self-end city-card__temp">
        {{ Math.round(city.weather.main.temp) }}&deg;
      </p>
      <div class="flex gap-2">
        <span class="text-xl">
          {{ Math.round(city.weather.main.temp_max) }}&deg; -
        </span>
        <span class="text-xl">
          {{ Math.round(city.weather.main.temp_min) }}&deg;
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  city: {
    type: Object,
    default: () => {},
  },
});
</script>

<style lang="scss">
  .city-card{
    flex-basis: 50%;
    box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.20);
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(50px);

    &__temp{
      font-size: 5vw;
      line-height: 95px;
    }

    &__h3{
      max-width: 75%;
      overflow: hidden;
    }
  }
</style>