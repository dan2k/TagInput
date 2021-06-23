<template>
  <h1>Tag Input</h1>
  <TagInput v-model="tags" />
  <ul>
    <li v-for="tag in tags" :key="tag">{{ tag }}</li>
  </ul>
</template> 

<script setup>
import { ref } from "vue";
import TagInput from "./components/TagInput.vue";
const tags = ref(['aaa','bbbb']);
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
ul li {
  text-align:left;
}
</style>