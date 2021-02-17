<template>
  <div class="carousel">
    <button @click="goLeft" class="left">-</button>
    <slot></slot>
    <button @click="goRight" class="right">+</button>
    <div class="indexT">
      <button
        v-for="(slide, index) in sliders"
        :key="index"
        @click="goGo(index)"
        :class="bgChange(index)"
        class="index-T"
      ></button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      index: 0,
      sliders: [],
      direction: null
    };
  },
  mounted() {
    this.sliders = this.$children;
    this.sliders.forEach((slide, i) => {
      slide.index = i;
    });
  },
  computed: {
    slideCount() {
      return this.sliders.length - 1;
    }
  },
  methods: {
    goLeft() {
      this.direction = "left";
      if (this.index > 0) {
        this.index -= 1;
      } else {
        this.index = this.slideCount;
      }
    },
    goRight() {
      this.direction = "right";
      if (this.index < this.slideCount) {
        this.index += 1;
      } else {
        this.index = 0;
      }
    },
    goGo(index) {
      this.direction = this.index < index ? "right" : "left";
      this.index = index;
    },
    bgChange(index) {
      if (index === this.index) {
        return "bg-color";
      }
    }
  }
};
</script>
<style lang="">
.carousel {
  position: relative;
  width: 100%;
  overflow: hidden;
}
.right {
  position: absolute;
  top: 50%;
  right: 15px;
}
.left {
  position: absolute;
  top: 50%;
  z-index: 1000;
  left: 15px;
}
.bg-color {
  background: rgb(0, 195, 255);
  border-color: rgb(0, 195, 255);
  outline: none;
  color: white;
}
.indexT {
  position: absolute;
  bottom: 15px;
  margin: 0 auto;
  left: 45%;
}
.index-T {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  margin: 0 10px;
}
</style>
