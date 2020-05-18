<template>
  <div v-if="getRainfall" class="rain">
    <span class="rain__value">
      {{ `${getRainfall} mm`.replace('.', ',') }}
    </span>
    <div class="rain__box" :style="{ height: `${getRainfall * 20}px` }"></div>
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
    getRainfall() {
      if ('rain' in this.weatherData.list[this.hour]) {
        return Math.round(this.weatherData.list[this.hour].rain['1h'] * 10) / 10;
      }
      return null;
    }
  }
};
</script>

<style lang="scss" scoped>
.rain {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;

  width: 100%;
  height: 100%;

  &__value {
    text-align: center;

    font-size: 16px;
  }

  &__box {
    background: #34ccff;
  }
}
</style>
