<template>
  <img alt="Vue logo" height="50" src="./assets/logo.png">
  <task-component/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TaskComponent from './components/TaskComponent.vue';

export default defineComponent({
  name: 'App',
  components: {
    TaskComponent
  }
});
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
