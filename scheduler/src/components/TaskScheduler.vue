<template>
  <div class="task-scheduler">
    <div class="left-panel">
      <AddWorkstationButton @add-workstation="addWorkstation" />
      <TaskPool :tasks="tasks" />
    </div>
    <div class="right-panel">
      <WorkstationPool
        :workstations="workstations"
        @assign-task="assignTaskToWorkstation"
      />
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
      tasks: [
        { id: 1, name: "任务A", hours: 2 },
        { id: 2, name: "任务B", hours: 4 },
        { id: 3, name: "任务C", hours: 3 },
      ],
      workstations: [
        { id: 1, name: "工位1", tasks: [] },
      ],
    };
  },
  methods: {
    addWorkstation() {
      this.workstations.push({
        id: this.workstations.length + 1,
        name: `工位${this.workstations.length + 1}`,
        tasks: [],
      });
    },
    assignTaskToWorkstation({ task, workstationId, hour }) {
      // 将任务从任务池中移除
      this.tasks = this.tasks.filter((t) => t.id !== task.id);

      // 将任务分配到对应的工位
      const workstation = this.workstations.find((ws) => ws.id === workstationId);
      workstation.tasks.push({ ...task, start: hour });
    },
  },
};
</script>

<style>
.task-scheduler {
  display: flex;
}

.left-panel {
  width: 20%;
  padding: 10px;
  border-right: 1px solid #ccc;
}

.right-panel {
  width: 80%;
  padding: 10px;
}
</style>
