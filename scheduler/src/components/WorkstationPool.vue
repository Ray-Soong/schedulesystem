<template>
  <div class="workstation-pool">
    <!-- 时间线，显示 9:00 到 19:00 (10 小时) -->
    <div class="time-line">
      <div class="time-line-label">时间</div>
      <div v-for="hour in 10" :key="hour" class="time-line-hour">
      {{ parseInt(hour) + 8 }}:00
      </div>
    </div>

    <!-- 工位行 -->
    <div v-for="workstation in workstations" :key="workstation.id" class="workstation-row">
      <div class="workstation-name">{{ workstation.name }}</div>
      <div class="workstation-timeline" @dragover.prevent @drop="onDrop($event, workstation.id)">
        <div v-for="hour in 10" :key="hour + 8" class="hour-block">
          <!-- 如果该时间段有任务 -->
          <div
            v-if="taskAtHour(workstation.tasks, hour + 8)"
            class="task-block"
            :style="taskStyle(taskAtHour(workstation.tasks, hour + 8))"
            draggable="true"
            @dragstart="onDragStart(taskAtHour(workstation.tasks, hour + 8))"
          >
            {{ taskAtHour(workstation.tasks, hour + 8).name }}
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
  methods: {
    onDragStart(task) {
      event.dataTransfer.setData("task", JSON.stringify(task));
    },
    onDrop(event, workstationId) {
      const task = JSON.parse(event.dataTransfer.getData("task"));
      const dropPosition = event.offsetX / event.target.clientWidth;
      const hour = Math.floor(dropPosition * 10) + 8;

      this.$emit("assign-task", { task, workstationId, hour });
    },
    taskAtHour(tasks, hour) {
      return tasks.find(task => task.start === hour);
    },
    taskStyle(task) {
      const start = (task.start - 8);
      return {
        width: `${task.hours * 100}%`,
        backgroundColor: "rgba(0, 123, 255, 0.8)",
        height: "90%",
        top: "5%",
        left: `${start}%`,
        position: "absolute",
      };
    },
  },
};
</script>

<style scoped>
.workstation-pool {
  display: flex;
  flex-direction: column;
}
.time-line {
  display: flex;
  margin-bottom: 15px;
  margin-left: var(--timeline-start, 100); /* 使用 CSS 变量配置起始位置 */
}
.time-line-hour {
  flex: 1; /* 保证每个小时格子宽度相等 */
  text-align: center;
  border-right: 1px solid #eee;
}
.time-line-label {
  width: var(--timeline-label-width, 100px); /* 使用 CSS 变量配置宽度 */
  text-align: center;
  font-weight: bold;
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
  border-right: 1px solid #eee;
  height: 40px;
  position: relative;
}
.task-block {
  background-color: rgba(0, 123, 255, 0.8);
  height: 90%;
  width: 100%;
  position: absolute;
  top: 5%;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: bold;
  text-align: center;
  border-radius: 5px;
}
</style>
