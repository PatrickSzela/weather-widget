<template>
  <div v-if="getDay" class="day">
    <span class="day__label">
      {{ getDay }}
    </span>
  </div>
</template>

<script>
export default {
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
  data() {
    return {
      days: ['Dzisiaj', 'Jutro', 'Pojutrze', 'Dzień 4', 'Dzień 5']
    };
  },
  computed: {
    getDay() {
      let ret = null;

      const hour = new Date(this.weatherData.list[this.hour].dt * 1000).getHours();
      if (hour === 0) {
        ret = this.days[Math.ceil(this.hour / 24)];
      }

      return ret;
    }
  }
};
</script>

<style lang="scss" scoped>
.day {
  width: 100%;
  padding: 0 10px;

  &__label {
    text-transform: uppercase;

    color: #888888;
  }
}
</style>
