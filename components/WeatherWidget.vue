<template>
  <div class="weather-widget">
    <div class="weather-widget__legend">
      <div
        v-for="(data, key) in displayData"
        :key="key"
        class="weather-widget__label weather-widget__row"
        :class="`weather-widget__row--${key}`"
      >
        {{ data.label }}
      </div>
    </div>

    <div class="weather-widget__scroll">
      <Glide class="weather-widget__data">
        <Slide v-for="(hourData, hour) in getHours" :key="hour" class="weather-widget__column">
          <div
            v-for="(data, key) in displayData"
            :key="key"
            class="weather-widget__row"
            :class="`weather-widget__row--${key}`"
          >
            <component :is="key" v-bind="{ hour: hour, weatherData: getWeatherData }"></component>
          </div>
        </Slide>
      </Glide>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import Glide, { Slide } from '~/components/Glide/Glide.vue';
import Day from '~/components/Day.vue';
import Hour from '~/components/Hour.vue';
import Condition from '~/components/Condition.vue';
import Temperature from '~/components/Temperature.vue';
import Rainfall from '~/components/Rainfall.vue';
import WindDirection from '~/components/WindDirection.vue';
import WindSpeed from '~/components/WindSpeed.vue';
import Pressure from '~/components/Pressure.vue';

import weatherData from '~/assets/weather_data.json';

export default Vue.extend({
  components: {
    Glide,
    Slide,
    Day,
    Hour,
    Condition,
    Temperature,
    Rainfall,
    WindDirection,
    WindSpeed,
    Pressure
  },
  data() {
    return {
      displayData: {
        day: { label: 'Dzień' },
        hour: { label: 'Godzina' },
        condition: { label: 'Prognoza' },
        temperature: { label: 'Temperatura' },
        rainfall: { label: 'Opady' },
        'wind-direction': { label: 'Kierunek wiatru' },
        'wind-speed': { label: 'Prędkość wiatru' },
        pressure: { label: 'Ciśnienie' }
      },
      dataFromChild: []
    };
  },
  computed: {
    getWeatherData() {
      return weatherData;
    },
    getHours() {
      return weatherData.list;
    }
  },
  created() {
    // inject min and max required for temperature and pressure graphs
    weatherData.city = {
      ...weatherData.city,
      ...this.getMinMax('temp'),
      ...this.getMinMax('pressure')
    };
  },
  methods: {
    getMinMax(type) {
      let lowest = weatherData.list[0].main[type];
      let highest = weatherData.list[0].main[type];

      weatherData.list.forEach((element) => {
        if (element.main[type] < lowest) {
          lowest = element.main[type];
        } else if (element.main[type] > highest) {
          highest = element.main[type];
        }
      });
      return { [`${type}_min`]: lowest, [`${type}_max`]: highest };
    }
  }
});
</script>

<style lang="scss" scoped>
.weather-widget {
  display: flex;
  overflow: hidden;

  &__column {
    flex: 0 0 90px;
  }

  &__row {
    display: flex;
    align-items: center;
    justify-content: center;

    &--day {
      height: 25px;
    }

    &--hour {
      height: 45px;
    }

    &--condition {
      height: 55px;
    }

    &--temperature {
      height: 165px;
    }

    &--rainfall,
    &--wind-direction {
      height: 90px;
    }
    &--wind-speed {
      height: 75px;
    }
    &--pressure {
      height: 150px;
    }
  }

  &__legend {
    z-index: 1;

    box-shadow: 6px 0 6px rgba(0, 0, 0, 0.1);
  }

  &__label {
    position: relative;

    padding: 4px 12px;

    text-align: center;

    color: #888888;
    border-bottom: solid 2px #eeeeee;
    background: white;

    font-size: 14px;

    &:last-child {
      border-bottom: none;
    }
  }

  &__data {
    display: flex;
    border-bottom: solid 1px #eeeeee;

    .weather-widget__row {
      border-left: solid 1px #eeeeee;

      &--day:empty {
        border-left: none;
      }

      &--wind-direction,
      &--wind-speed {
        border-left: solid 1px #ffffff;
        background: #eeeeee;
      }
    }
  }

  &__scroll {
    overflow: hidden;
  }
}
</style>
