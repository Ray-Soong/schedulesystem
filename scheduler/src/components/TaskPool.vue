<template>
    <div class="task-pool">
      <h3>任务池</h3>
      <div
        v-for="task in tasks"
        :key="task.id"
        class="task-item"
        draggable="true"
        @dragstart="onDragStart(task)"
      >
        {{ task.name }} ({{ task.hours }}小时)
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "TaskPool",
    data() {
      return {
        tasks: [
          { id: 1, name: "任务A", hours: 2 },
          { id: 2, name: "任务B", hours: 4 },
          { id: 3, name: "任务C", hours: 3 },
        ],
      };
    },
    methods: {
      onDragStart(task) {
        event.dataTransfer.setData("task", JSON.stringify(task));
      },
    },
  };
  </script>
  
  <style>
  .task-pool {
    border: 1px solid #ccc;
    padding: 10px;
  }
  .task-item {
    margin: 10px;
    padding: 5px;
    background-color: #f0f0f0;
    cursor: move;
  }
  </style>
  