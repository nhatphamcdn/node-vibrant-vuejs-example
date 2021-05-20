<template>
  <div id="app">
    <h2>Extract prominent colors from an image using library <br /> <a href="https://github.com/Vibrant-Colors/node-vibrant">node-vibrant</a></h2>
    <image-pickup />
  </div>
</template>

<script>
import ImagePickup from './components/ImagePickup';

export default {
  name: 'App',
  components: {
    ImagePickup
  }
}
</script>

<style>
body {
  background: #eee;
  font-family: 'Montserrat', sans-serif;
}

h2 {
  text-align: center;
  font-size: 2.75rem;
}
</style>
