<template>
  <div class="condition">
    <img class="condition__image" :src="getConditionIcon" />
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
  computed: {
    getConditionIcon() {
      return `http://openweathermap.org/img/wn/${this.weatherData.list[this.hour].weather[0].icon}@2x.png`;
    }
  }
};
</script>

<style lang="scss" scoped>
.condition {
  position: relative;

  overflow: hidden;

  width: 100%;
  height: 100%;

  &__image {
    position: absolute;
    top: 50%;
    left: 50%;

    width: 100%;

    transform: translate(-50%, -50%);
  }
}
</style>
