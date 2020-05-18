<template>
  <div ref="graph" class="graph">
    <div class="graph__inner" :style="{ top: `${calculateYOffset(value)}%` }">
      <slot></slot>

      <div v-if="nextValue !== null" class="graph__line" :style="style"></div>

      <div class="graph__circle"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      required: true
    },
    nextValue: {
      type: Number,
      default: null
    },
    min: {
      type: Number,
      required: true
    },
    max: {
      type: Number,
      required: true
    },
    lineColor: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      style: {}
    };
  },
  mounted() {
    this.style = this.calculateLineStyle();
  },
  methods: {
    calculateYOffset(value) {
      return ((this.max - value) / (this.max - this.min)) * 100;
    },
    calculateLineStyle() {
      if (!this.$refs.graph) {
        return {};
      }

      // https://css-tricks.com/how-to-make-a-line-chart-with-css/
      const size = this.$refs.graph.getBoundingClientRect();

      const offset = this.calculateYOffset(this.nextValue);
      const offsetRelative = this.calculateYOffset(this.value);

      const heightDiff = ((offset - offsetRelative) / 100) * size.height;
      const hypotenuse = Math.sqrt(heightDiff ** 2 + size.width ** 2);
      const angle = (Math.asin(heightDiff / hypotenuse) * 180) / Math.PI;

      return {
        transform: `rotate(${angle}deg)`,
        width: `${hypotenuse}px`,
        backgroundColor: this.lineColor
      };
    }
  }
};
</script>

<style lang="scss" scoped>
$circle_size: 14px;
$line_height: 2px;

.graph {
  position: relative;

  width: 100%;
  height: 100%;

  &__inner {
    position: absolute;

    display: flex;
    align-items: center;
    flex-direction: column;

    width: 100%;

    transform: translateY(-50%);
  }

  &__circle {
    z-index: 1;

    width: $circle_size;
    height: $circle_size;

    border: solid 2px black;
    border-radius: 50%;
    background: white;
  }

  &__line {
    position: absolute;
    bottom: ($circle_size / 2) - ($line_height / 2);
    left: 50%;

    height: $line_height;

    transform-origin: left center;

    background: black;
  }
}
</style>
