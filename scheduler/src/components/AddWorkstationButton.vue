<template>
    <button @click="addWorkstation">添加工位</button>
  </template>
  
  <script>
  export default {
    name: "AddWorkstationButton",
    methods: {
      addWorkstation() {
        this.$emit("add-workstation");
      },
    },
  };
  </script>
  