<template>
  <div class="workstation-pool">
    <div v-for="workstation in workstations" :key="workstation.id" class="workstation-row">
      <div class="workstation-name">{{ workstation.name }}</div>
      <div class="workstation-timeline" @dragover.prevent @drop="onDrop($event, workstation.id)">
        <div v-for="hour in 9" :key="hour" class="hour-block">
          {{ hour + 9 }}:00
          <div
            v-for="task in filteredTasks(workstation.tasks, hour)"
            :key="task.id"
            class="task-block"
            :style="{ width: taskWidth(task.hours) + '%' }"
          >
            {{ task.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WorkstationPool",
  props: {
    workstations: Array,
  },
  computed: {
    filteredTasks() {
      return (tasks, hour) => {
        return tasks.filter((task) => task.start === hour);  // 过滤特定时间段的任务
      };
    },
  },
  methods: {
    onDrop(event, workstationId) {
      const task = JSON.parse(event.dataTransfer.getData("task"));
      const hour = Math.floor(event.offsetX / (event.target.clientWidth / 9)) + 9;
      this.$emit("assign-task", { task, workstationId, hour });
    },
    taskWidth(hours) {
      return (hours / 9) * 100;
    },
  },
};
</script>

<style>
.workstation-pool {
  display: flex;
  flex-direction: column;
}

.workstation-row {
  display: flex;
  margin-bottom: 10px;
}

.workstation-name {
  width: 100px;
  text-align: center;
  font-weight: bold;
}

.workstation-timeline {
  flex-grow: 1;
  display: flex;
  border: 1px solid #ccc;
}

.hour-block {
  flex-grow: 1;
  text-align: center;
  position: relative;
  border-right: 1px solid #eee;
}

.task-block {
  position: absolute;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  box-sizing: border-box;
  left: 0;
  top: 0;
  height: 100%;
  z-index: 1;
}
</style>
