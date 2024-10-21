<template>
  <div class="workstation-pool">
    <!-- 时间线，统一显示在所有工位上方 -->
    <div class="time-line">
      <div v-for="hour in 9" :key="hour" class="time-line-hour">
        {{ hour + 9 }}:00
      </div>
    </div>

    <!-- 工位行 -->
    <div v-for="workstation in workstations" :key="workstation.id" class="workstation-row">
      <div class="workstation-name">{{ workstation.name }}</div>
      <div class="workstation-timeline" @dragover.prevent @drop="onDrop($event, workstation.id)">
        <!-- 仅显示网格，不显示具体时间 -->
        <div v-for="hour in 9" :key="hour" class="hour-block">
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
      return (tasks, hour) => tasks.filter((task) => task.start === hour);
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

.time-line {
  display: flex;
  margin-bottom: 10px;
}

.time-line-hour {
  flex-grow: 1;
  text-align: center;
  border-right: 1px solid #eee;
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
  position: relative;
}

.hour-block {
  flex-grow: 1;
  position: relative;
  border-right: 1px solid #eee;
  height: 40px;
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
