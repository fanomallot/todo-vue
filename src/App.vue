<template>
  <div id="app" >
    <todos v-model="todos" />
    <todos />
    <carousel>
      <carouselslide>
        <img
          src="https://images.pexels.com/photos/2887718/pexels-photo-2887718.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260"
          alt="photo"
        />
      </carouselslide>
      <carouselslide>
        <img
          src="https://images.pexels.com/photos/6551925/pexels-photo-6551925.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500"
          alt="photo"
        />
      </carouselslide>
    </carousel>
  </div>
</template>

<script>
import Carousel from "./components/carousel/carousel.vue";
import Carouselslide from "./components/carousel/carouselslide.vue";
import HelloWorld from "./components/HelloWorld";
import Todos from "./components/todos.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    Todos,
    Carousel,
    Carouselslide
  },
  data() {
    return {
      todos: [
        {
          name: "test",
          completed: true
        }
      ]
    };
  }
};
</script>

<style scoped>
img {
  width: 100%;
  height: 250px;
  object-fit: cover;
}
</style>
