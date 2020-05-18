<template>
  <div class="wind-direction">
    <img
      class="wind-direction__arrow"
      src="~/assets/images/wind_arrow.svg"
      :style="{ transform: `rotate(${getRotation + 180}deg)` }"
    />
    <span class="wind-direction__label">{{ getDirection }}</span>
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
      directions: ['Północny', 'Pn.-Wsch.', 'Wschodni', 'Pd.-Wsch.', 'Południowy', 'Pd.-Zach.', 'Zachodni', 'Pn.-Zach.']
    };
  },
  computed: {
    getRotation() {
      return this.weatherData.list[this.hour].wind.deg;
    },
    getDirection() {
      const index = Math.floor((this.weatherData.list[this.hour].wind.deg + 22.5) / 45);
      return this.directions[index > 7 ? 0 : index];
    }
  }
};
</script>

<style lang="scss" scoped>
.wind-direction {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: flex-end;

  width: 100%;
  height: 100%;

  &__label {
    margin: 12px 0 4px;

    font-size: 15px;
  }

  &__arrow {
    height: 40%;
  }
}
</style>
