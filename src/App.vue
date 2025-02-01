<template>
  <div id="app">
    <CarouselContainer />
  </div>
</template>

<script>
import CarouselContainer from './components/CarouselContainer.vue';

export default {
  components: {
    CarouselContainer
  }
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 20px;
}

h1 {
  color: #333;
  margin-bottom: 30px;
}
</style>