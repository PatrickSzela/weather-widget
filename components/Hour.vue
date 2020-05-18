<template>
  <div class="hour">
    <span>
      {{ getHour }}
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
  computed: {
    getHour() {
      const date = new Date(this.weatherData.list[this.hour].dt * 1000);
      const options = {
        hour: 'numeric',
        minute: 'numeric'
      };
      return new Intl.DateTimeFormat('pl-PL', options).format(date);
    }
  }
};
</script>

<style lang="scss" scoped>
.hour {
  display: flex;
  align-items: center;
  justify-content: center;

  width: 100%;
  height: 100%;

  text-align: center;
  vertical-align: middle;

  font-size: 20px;
  font-weight: 500;
}
</style>
