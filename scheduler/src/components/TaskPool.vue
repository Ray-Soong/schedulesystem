<template>
  <div class="task-pool">
    <h3>任务池</h3>
    <button @click="showAddTaskModal = true">添加任务</button>
    <div
      v-for="task in tasks"
      :key="task.id"
      class="task-item"
      :style="{ width: taskWidth(task.hours) + '%' }"
      draggable="true"
      @dragstart="onDragStart(task)"
    >
      {{ task.name }} ({{ task.hours }}小时)
      <button @click="deleteTask(task.id)">删除</button>
    </div>

    <div v-if="showAddTaskModal" class="modal">
      <div class="modal-content">
        <h3>添加任务</h3>
        <label>任务名称:</label>
        <input v-model="newTask.name" type="text" />
        <label>任务时长:</label>
        <input v-model.number="newTask.hours" type="number" min="1" max="8" />
        <button @click="addTask">确认</button>
        <button @click="showAddTaskModal = false">取消</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskPool",
  props: {
    tasks: Array, // 从父组件接收任务列表
  },
  data() {
    return {
      showAddTaskModal: false,
      newTask: { name: "", hours: 1 },
    };
  },
  methods: {
    onDragStart(task) {
      event.dataTransfer.setData("task", JSON.stringify(task));
    },
    taskWidth(hours) {
      return (hours / 8) * 100;
    },
    addTask() {
      if (this.newTask.name && this.newTask.hours > 0 && this.newTask.hours <= 8) {
        const newTask = { ...this.newTask, id: Date.now() };
        this.$emit("add-task", newTask);
        this.newTask.name = "";
        this.newTask.hours = 1;
        this.showAddTaskModal = false;
      }
    },
    deleteTask(taskId) {
      this.$emit("delete-task", taskId);
    },
  },
};
</script>

