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
      {{ task.name }} ({{ task.hours }}小时)
      <button @click="deleteTask(task.id)">删除</button>
    </div>

    <!-- 添加任务的弹框 -->
    <div v-if="showAddTaskModal" class="modal">
      <div class="modal-content">
        <h3>添加任务</h3>
        <label>
          任务名称:
          <input v-model="newTask.name" type="text" />
        </label>
        <label>
          任务时长:
          <input v-model.number="newTask.hours" type="number" min="1" max="8" />
        </label>
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
    tasks: Array, // 父组件传递的任务列表
  },
  data() {
    return {
      showAddTaskModal: false, // 控制任务添加弹框的显示
      newTask: {
        name: "", // 新任务名称
        hours: 1, // 新任务时长，默认1小时
      },
    };
  },
  methods: {
    // 处理任务拖动事件
    onDragStart(task) {
      event.dataTransfer.setData("task", JSON.stringify(task));
    },

    // 计算任务在任务池中的宽度，根据任务时长显示不同的宽度
    taskWidth(hours) {
      return (hours / 8) * 100; // 任务的时长在 1 到 8 小时之间
    },

    // 添加任务到任务池中，并通过 emit 将新任务传递给父组件
    addTask() {
      if (this.newTask.name && this.newTask.hours > 0 && this.newTask.hours <= 8) {
        const newTask = { ...this.newTask, id: Date.now() }; // 创建新任务并生成唯一ID
        this.$emit("add-task", newTask); // 通知父组件添加任务
        this.newTask.name = ""; // 重置任务名称输入框
        this.newTask.hours = 1; // 重置任务时长输入框
        this.showAddTaskModal = false; // 关闭弹框
      }
    },

    // 删除任务，通过 emit 通知父组件删除任务
    deleteTask(taskId) {
      this.$emit("delete-task", taskId); // 通知父组件删除任务
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
  box-sizing: border-box;
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
