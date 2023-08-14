<template>
  <div id="app">
    <OptionalProduct />
    <!-- <DragAndDrop /> -->
  </div>
</template>

<script>
import OptionalProduct from './components/OptionalProduct.vue'
// import DragAndDrop from './components/DragAndDrop.vue'

export default {
  name: 'App',
  components: {
    OptionalProduct,
    // DragAndDrop
  }
}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
