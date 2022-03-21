<template>
  <TodoList/>
</template>

<script>
import TodoList from './components/ToList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
#app {
  height: 100vh;
  width: 100vw;
}
</style>
