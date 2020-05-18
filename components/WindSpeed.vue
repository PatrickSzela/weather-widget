<template>
  <div class="wind-direction">
    <span class="wind-direction__description">{{ getDescription }}</span>
    <span class="wind-direction__speed">{{ getSpeed }} km/h</span>
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
      descriptions: ['Słaby', 'Umiar.', 'Silny']
    };
  },
  computed: {
    getSpeed() {
      return Math.round(this.weatherData.list[this.hour].wind.speed * 3.6);
    },
    getDescription() {
      const index = Math.floor(this.getSpeed / 15);
      return this.descriptions[index > 2 ? 2 : index];
    }
  }
};
</script>

<style lang="scss" scoped>
.wind-direction {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;

  width: 100%;
  height: 100%;

  &__description {
    margin-bottom: 6px;

    font-size: 15px;
  }

  &__speed {
    font-size: 17px;
  }
}
</style>
