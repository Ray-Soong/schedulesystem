<template>
  <div class="task-pool">
    <h3>任务池</h3>
    <!-- 添加任务按钮 -->
    <button @click="showAddTaskModal = true">添加任务</button>

    <!-- 任务列表 -->
    <div
      v-for="task in tasks"
      :key="task.id"
      class="task-item"
      :style="{ width: taskWidth(task.hours) + '%' }"
      draggable="true"
      @dragstart="onDragStart(task)"
    >
      {{ task.name }} ({{ task.hours }}H)
      <button @click="deleteTask(task.id)">删除</button>
    </div>

    <!-- 添加任务的弹框 -->
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
    tasks: Array,
  },
  data() {
    return {
      showAddTaskModal: false,
      newTask: {
        name: "",
        hours: 1,
      },
    };
  },
  methods: {
    onDragStart(task) {
      event.dataTransfer.setData("task", JSON.stringify(task));
    },
    taskWidth(hours) {
      return (hours / 8) * 100; // 任务时长控制宽度
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

<style scoped>
.task-pool {
  border: 1px solid #ccc;
  padding: 10px;
  display: flex;
  flex-direction: column;
}
.task-item {
  margin: 10px 0;
  padding: 5px;
  background-color: #f0f0f0;
  cursor: move;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
}
button {
  margin-left: 10px;
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
</style>
