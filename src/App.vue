<template>
  <div class="app">
    <h1>Task Management App</h1>
    <task-list />
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

export default {
  name: 'App',
  components: {
    TaskList,
  },
};
</script>

<style scoped>
.app {
  text-align: center;
  padding: 20px;
  font-family: Arial, sans-serif;
  color: #e0e0e0;
  background-color: #1c1c1c;
  min-height: 100vh;
}
h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
}
</style>
