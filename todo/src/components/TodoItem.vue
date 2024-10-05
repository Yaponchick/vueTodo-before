<template>
    <div>
      {{ index }} - {{ todo.title }}
      <input type="checkbox" :checked="todo.completed" @change="complete" />
    </div>
  </template>
  
  <script>
  
  export default {
    props: ["todo", "index"],
    emits: ["complete-done"],
    methods: {
      complete() {
        this.$emit("complete-done");
      }
    }
  };
  </script>
  
  <style scoped>
  </style>
  