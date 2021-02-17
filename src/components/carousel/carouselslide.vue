<template lang="">
  <transition :name="transition">
    <div v-show="visible">
      <slot></slot>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      index: 0
    };
  },
  computed: {
    transition() {
      return "slide-" + this.$parent.direction;
    },
    visible() {
      return this.index === this.$parent.index;
    }
  }
};
</script>
<style lang="">
.slide-right-enter-active {
  animation: slidrightIn 2s;
}
.slide-right-leave-active {
  animation: slidrightOut 2s;
  position: absolute;
  top: 0px;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
}
@keyframes slidrightIn {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}
@keyframes slidrightOut {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-100%);
  }
}
.slide-left-enter-active {
  animation: slidleftIn 2s;
}
.slide-left-leave-active {
  animation: slidleftOut 2s;
  position: absolute;
  top: 0px;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
}
@keyframes slidleftIn {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}
@keyframes slidleftOut {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(100%);
  }
}
</style>
