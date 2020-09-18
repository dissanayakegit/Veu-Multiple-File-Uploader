<template>
  <div id="app">
    <!-- <ImageUploader /> -->
    <ImageSlider />
  </div>
</template>

<script>
// import ImageUploader from './components/ImageUploader.vue'
import ImageSlider from './components/ImageSlider'

export default {
  name: 'App',
  components: {
    // ImageUploader,
    ImageSlider
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
