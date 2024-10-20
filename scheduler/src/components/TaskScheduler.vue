<template>
  <div class="task-scheduler">
    <div class="left-panel">
      <AddWorkstationButton @add-workstation="addWorkstation" />
      <button @click="addTask">添加任务</button>
      <TaskPool />
    </div>
    <div class="right-panel">
      <WorkstationPool v-for="workstation in workstations" :key="workstation.id" />
    </div>
  </div>
</template>

<script>
import TaskPool from "./TaskPool.vue";
import WorkstationPool from "./WorkstationPool.vue";
import AddWorkstationButton from "./AddWorkstationButton.vue";

export default {
  name: "TaskScheduler",
  components: { TaskPool, WorkstationPool, AddWorkstationButton },
  data() {
    return {
      workstations: [],
    };
  },
  methods: {
    addWorkstation() {
      this.workstations.push({
        id: this.workstations.length + 1,
        name: `工位 ${this.workstations.length + 1}`,
      });
    },
    addTask() {
      // 添加任务的逻辑
    },
  },
};
</script>
