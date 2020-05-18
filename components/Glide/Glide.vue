<template>
  <div ref="glide" class="glide" @mouseenter="showButtons = true" @mouseleave="showButtons = false">
    <div class="glide__track" data-glide-el="track">
      <ul class="glide__slides">
        <slot></slot>
      </ul>
    </div>
    <div class="glide__arrows" :class="!showButtons ? 'glide__arrows--hidden' : ''" data-glide-el="controls">
      <button
        class="glide__arrow glide__arrow--left"
        :class="!showButtonLeft ? 'glide__arrow--hidden' : ''"
        data-glide-dir="<"
      ></button>

      <button
        class="glide__arrow glide__arrow--right"
        :class="!showButtonRight ? 'glide__arrow--hidden' : ''"
        data-glide-dir=">"
      ></button>
    </div>

    <div class="glide__scrollbar" :class="!showScrollbar ? 'glide__scrollbar--hidden' : ''">
      <div ref="scrollbarThumb" class="glide__scrollbar_thumb" :style="scrollbarThumbStyle"></div>
    </div>
  </div>
</template>

<script>
import Glide from '@glidejs/glide';
import Slide from '~/components/Glide/Slide.vue';

export default {
  data() {
    return {
      glide: null,
      options: {
        startAt: 0,
        gap: 0,
        rewind: false,
        touchRatio: 1,
        perView: 0,
        bound: true,
        swipeThreshold: 45,
        dragThreshold: 45
      },
      resizeObserver: null,
      showButtons: false,
      showButtonLeft: true,
      showButtonRight: true,
      showScrollbar: false,
      scrollbarThumbStyle: {},
      onMoveTimeout: null
    };
  },
  mounted() {
    this.resizeObserver = new ResizeObserver(() => {
      // this.glide.update(this.createOptions()) // updating doesn't handle changing perView very well...
      if (this.glide) {
        this.glide.destroy();
      }

      this.options = { ...this.options, ...this.calculateOptions() };
      this.glide = new Glide('.glide', this.options).mount();

      this.glide.on('move', this.onMove);
      this.showHideButtons();
    });

    this.resizeObserver.observe(this.$refs.glide);
  },
  beforeDestroy() {
    this.resizeObserver.disconnect();

    if (this.glide) {
      this.glide.destroy();
    }
  },
  methods: {
    onMove(e) {
      this.showHideButtons();
      this.calculateScrollbar(e);
    },
    calculateOptions() {
      const options = {};

      if (this.$refs) {
        const glideSize = this.$refs.glide.getBoundingClientRect();
        const slideSize = this.$refs.glide.querySelector('.glide__slide').getBoundingClientRect();

        options.perView = glideSize.width / slideSize.width;
        options.dragThreshold = slideSize.width / 2;
        options.swipeThreshold = options.dragThreshold;

        if (this.glide) {
          options.startAt = this.glide.index;
        }
      }

      return options;
    },
    showHideButtons() {
      this.showButtonLeft = true;
      this.showButtonRight = true;

      if (this.glide.index === 0) {
        this.showButtonLeft = false;
      }
      if (
        this.glide.index ===
        Math.ceil(this.$refs.glide.querySelector('.glide__slides').childElementCount - this.options.perView)
      ) {
        this.showButtonRight = false;
      }
    },
    calculateScrollbar(e) {
      const scrollerWidth = this.$refs.glide.querySelector('.glide__track').getBoundingClientRect().width;
      const scrollWidth = this.$refs.glide.querySelector('.glide__slides').getBoundingClientRect().width;
      const thumbWidth = (scrollerWidth * scrollerWidth) / scrollWidth;
      const factor = (scrollerWidth - thumbWidth) / (scrollWidth - scrollerWidth);

      this.scrollbarThumbStyle = {
        transform: `translateX(${e.movement * factor}px)`,
        width: `${thumbWidth}px`
      };
      this.showScrollbar = true;

      if (this.onMoveTimeout) {
        clearTimeout(this.onMoveTimeout);
      }
      this.onMoveTimeout = setTimeout(() => {
        this.showScrollbar = false;
      }, 1000);
    }
  }
};

export { Slide };
</script>

<style lang="scss" scoped>
@import 'node_modules/@glidejs/glide/src/assets/sass/glide.core';

$arrow_size: 90px;

.glide {
  &--swipeable {
    cursor: grab;
  }

  &--dragging {
    cursor: grabbing;
  }

  &__track {
    mask-image: linear-gradient(to right, black 80%, transparent);
  }

  &__slides {
    margin: 0;
  }

  &__arrows {
    position: absolute;
    top: 50%;

    display: flex;
    overflow: hidden;

    width: 100%;
    height: $arrow_size;

    transition: opacity 0.5s;
    transform: translateY(-50%);
    pointer-events: none;

    &--hidden {
      opacity: 0;
    }
  }

  &__arrow {
    position: absolute;

    display: flex;

    width: $arrow_size;
    height: $arrow_size;

    cursor: pointer;
    transition: opacity 0.5s;
    pointer-events: all;

    border: none;
    border-radius: 50%;
    outline: none;
    background: rgba(0, 0, 0, 0.15);

    &:hover {
      background: rgba(0, 0, 0, 0.2);
    }

    &::after {
      position: absolute;
      top: 50%;

      width: 0;
      height: 0;

      content: '';
      transform: translateY(-50%);

      border-top: 10px solid transparent;
      border-bottom: 10px solid transparent;
    }

    &--hidden {
      pointer-events: none;

      opacity: 0;
    }

    &--left {
      left: 0;

      transform: translateX(-50%);

      &::after {
        right: 18px;

        border-right: 16px solid white;
      }
    }

    &--right {
      right: 0;

      transform: translateX(50%);

      &::after {
        left: 18px;

        border-left: 16px solid white;
      }
    }
  }

  &__scrollbar {
    position: absolute;
    bottom: 0;

    width: 100%;
    height: 3px;

    transition: opacity 0.5s;

    background: #f0f0f0;

    &--hidden {
      opacity: 0;
    }

    &_thumb {
      height: 100%;

      transition: transform 0.15s linear;

      background: #5e605e;

      .glide--dragging & {
        transition: none;
      }
    }
  }
}
</style>
