<template>
  <div class="task-scheduler">
    <div class="left-panel">
      <!-- 任务池 -->
      <TaskPool :tasks="tasks" @add-task="addTask" @delete-task="deleteTask" />

      <!-- 添加工位按钮 -->
      <AddWorkstationButton @add-workstation="addWorkstation" />

      <!-- 统计任务信息 -->
      <TaskStats :tasks="tasks" :workstations="workstations" />
    </div>

    <!-- 工位池 -->
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
import TaskStats from "./TaskStats.vue";

export default {
  name: "TaskScheduler",
  components: { TaskPool, WorkstationPool, AddWorkstationButton , TaskStats },
  data() {
    return {
      tasks: [
        { id: 1, name: "拼装凳子", hours: 6 },
        { id: 2, name: "刷漆", hours: 4 },
        { id: 3, name: "抛光", hours: 5 },
      ],
      workstations: [
        { id: 1, name: "工位1", tasks: [] },
        { id: 2, name: "工位2", tasks: [] },
      ],
    };
  },
  methods: {
    addWorkstation(name) {
      this.workstations.push({
        id: this.workstations.length + 1,
        name,
        tasks: [],
      });
    },
    addTask(task) {
      this.tasks.push(task);
    },
    deleteTask(taskId) {
      this.tasks = this.tasks.filter((task) => task.id !== taskId);
    },
    assignTaskToWorkstation({ task, workstationId, hour }) {
      const workstation = this.workstations.find((ws) => ws.id === workstationId);
      const endHour = hour + task.hours;

      // 检查时间段是否有重叠任务
      const hasOverlap = workstation.tasks.some(
        (t) => (t.start < endHour && t.start >= hour)
      );

      if (!hasOverlap) {
        // 从任务池或原工位中移除任务
        this.tasks = this.tasks.filter((t) => t.id !== task.id);
        this.workstations.forEach((ws) => {
          ws.tasks = ws.tasks.filter((t) => t.id !== task.id);
        });

        // 将任务添加到目标工位
        workstation.tasks.push({ ...task, start: hour });
      }
    },
  },
};
</script>

<style scoped>
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
