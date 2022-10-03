<template>
  <!-- Banner click plus -->
  <div
      v-if="route.query.preview"
      class="text-black p-4 border w-full text-center city-view__banner"
  >
    <p>
      You are currently previewing this city, click the "+"
      icon to start tracking this city.
    </p>
  </div>

  <div class="items-center city-view container">

    <!-- Weather Overview -->
    <div class="flex flex-col flex-1 items-center text-black city-view__overview">
      <h1 class="city-view__h1">{{ route.params.city }}</h1>
      <p class="city-view__sub-t">
        {{
          new Date(weatherData.currentTime).toLocaleDateString(
            "en-us",
            {
              weekday: "short",
              day: "2-digit",
              month: "long",
            }
          )
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString(
            "en-us",
            {
              timeStyle: "short",
            }
          )
        }}
      </p>
      <p class="city-view__main-t">
        {{ Math.round(weatherData.current.temp) }}&deg;
      </p>
      <p class="city-view__feels-like">
        Feels like
        {{ Math.round(weatherData.current.feels_like) }} &deg;
      </p>
      <p class="capitalize city-view__feels-like">
        {{ weatherData.current.weather[0].description }}
      </p>
      <img
        class="w-[150px] h-auto"
        :src="
          `http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`
        "
        alt=""
      />
      <div
          class="city-view__remove-city flex items-center gap-2 text-black cursor-pointer duration-150 hover:text-red-500"
          @click="removeCity"
      >
        <i class="fa-solid fa-trash"></i>
        <p>Remove City</p>
      </div>
    </div>

    <div class="city-view__box">
      <!-- Hourly Weather -->
      <div class="city-view__hourly max-w-screen-md w-full">
        <div class="px-10 py-10 text-black">
          <h2 class="city-view__hourly-h2 mb-2">Hourly Weather</h2>
          <div class="flex gap-10 overflow-x-scroll">
            <div
                v-for="hourData in weatherData.hourly"
                :key="hourData.dt"
                class="flex flex-col gap-1 items-center"
            >
              <p class="whitespace-nowrap text-md city-view__hourly-t">
                {{
                  new Date(
                      hourData.currentTime
                  ).toLocaleTimeString("en-us", {
                    hour: "numeric",
                  })
                }}
              </p>
              <img
                  class="w-auto h-[50px] object-cover"
                  :src="
                    `http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`
                  "
                  alt=""
              />
              <p class="city-view__hourly-t">
                {{ Math.round(hourData.temp) }}&deg;
              </p>
            </div>
          </div>
        </div>
      </div>

      <!-- Weekly Weather -->
      <div class="city-view__weekly max-w-screen-md w-full">
        <div class="px-10 py-10 text-black">
          <h2 class="city-view__weekly-h2">7 Day Forecast</h2>
          <div
              v-for="day in weatherData.daily"
              :key="day.dt"
              class="flex items-center"
          >
            <p class="flex-1">
              {{
                new Date(day.dt * 1000).toLocaleDateString(
                    "en-us",
                    {
                      weekday: "long",
                    }
                )
              }}
            </p>
            <img
                class="w-[40px] h-[40px] object-cover"
                :src="
                  `http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`
                "
                alt=""
            />
            <div class="flex gap-2 flex-1 justify-end">
              <p>{{ Math.round(day.temp.max) }}&deg; -</p>
              <p>{{ Math.round(day.temp.min) }}&deg;</p>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute, useRouter } from "vue-router";
import VideoBackground from "./VideoBackground.vue";

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
    );

    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 3 * weatherData.data.timezone_offset;

    // cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime =
        utc + 3 * weatherData.data.timezone_offset;
    });

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
const weatherData = await getWeatherData();

const router = useRouter();
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("savedCities"));
  const updatedCities = cities.filter(
    (city) => city.id !== route.query.id
  );
  localStorage.setItem(
    "savedCities",
    JSON.stringify(updatedCities)
  );
  router.push({
    name: "home",
  });
};
</script>

<style lang="scss">
  .city-view{
    display: grid;
    grid-template-columns: 30% 67%;
    gap: 40px;

    &__h1{
      font-size: 3vw;
      font-weight: bold;
    }

    &__sub-t{
      font-size: 1vw;
    }

    &__banner{
      background: rgba(255, 255, 255, 0.3);
      backdrop-filter: blur(35px);
      z-index: 1;
    }

    &__overview{
      padding: 64px 30px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.25);
      backdrop-filter: blur(35px);
      border-radius: 16px;
      align-self: baseline;
    }

    &__main-t{
      font-size: 8vw;
    }

    &__feels-like{
      font-size: 1vw;
    }

    &__remove-city{
      background: rgba(255, 255, 255, 0.5);
      backdrop-filter: blur(100px);
      padding: 8px 24px;
      border-radius: 80px;
    }

    &__box{
      display: flex;
      flex-direction: column;
    }

    &__hourly{
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.25);
      backdrop-filter: blur(100px);
      border-radius: 16px;
      margin: 0 auto 40px;

      &-h2{
        font-size: 32px;
      }

      &-t{
        font-size: 16px;
      }
    }

    &__weekly{
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.25);
      backdrop-filter: blur(100px);
      border-radius: 16px;

      &-h2{
        font-size: 32px;
        margin: 0 auto 16px;
      }
    }
  }
</style>