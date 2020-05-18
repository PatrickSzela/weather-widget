<template>
  <div class="temperature">
    <graph
      :value="weatherData.list[hour].main.temp"
      :next-value="weatherData.list[hour + 1] ? weatherData.list[hour + 1].main.temp : null"
      :min="weatherData.city.temp_min"
      :max="weatherData.city.temp_max"
      line-color="#ffd302"
    >
      <span class="temperature__text">{{ getCurrent }}°</span>
    </graph>
  </div>
</template>

<script>
import Graph from '~/components/Graph';

export default {
  components: {
    Graph
  },
  props: {
    hour: {
      type: Number,
      required: true
    },
    weatherData: {
      type: Object,
      required: true
    }
  },
  computed: {
    getCurrent() {
      return this.getTemperature(this.hour);
    }
  },
  methods: {
    getTemperature(hour) {
      return Math.round(this.kelvinToCelsius(this.weatherData.list[hour].main.temp));
    },
    kelvinToCelsius(temp) {
      return temp - 273.15;
    }
  }
};
</script>

<style lang="scss" scoped>
.temperature {
  width: 100%;
  height: 100%;
  padding: 24px 0;

  &__text {
    font-size: 24px;
    font-weight: 500;
  }
}
</style>
