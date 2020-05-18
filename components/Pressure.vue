<template>
  <div class="pressure">
    <graph
      :value="weatherData.list[hour].main.pressure"
      :next-value="weatherData.list[hour + 1] ? weatherData.list[hour + 1].main.pressure : null"
      :min="weatherData.city.pressure_min"
      :max="weatherData.city.pressure_max"
    >
      <span class="pressure__text">{{ getCurrent }} hPa</span>
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
      return this.getPressure(this.hour);
    }
  },
  methods: {
    getPressure(hour) {
      return Math.round(this.weatherData.list[hour].main.pressure);
    }
  }
};
</script>

<style lang="scss" scoped>
.pressure {
  width: 100%;
  height: 100%;
  padding: 24px 0;

  &__text {
    font-size: 16px;
  }
}
</style>
