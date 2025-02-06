<template>
  <TodoComponent />
</template>

<script>
import Navbar from './components/Navbar.vue';
import TodoComponent from './components/TodoComponent.vue';

export default {
  name: 'App',
  components: {
    TodoComponent,
    Navbar
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');

body {
  font-family: 'Quicksand', sans-serif !important;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
}
</style>