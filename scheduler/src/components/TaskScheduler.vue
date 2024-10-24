<template>
  <div class="task-scheduler">
    <div class="left-panel">
      <!-- 添加工位按钮 -->
      <AddWorkstationButton @add-workstation="addWorkstation" />

      <!-- 任务池 -->
      <TaskPool :tasks="tasks" @add-task="addTask" @delete-task="deleteTask" />
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
// 导入相关组件
import TaskPool from "./TaskPool.vue";
import WorkstationPool from "./WorkstationPool.vue";
import AddWorkstationButton from "./AddWorkstationButton.vue";

export default {
  name: "TaskScheduler",
  components: { TaskPool, WorkstationPool, AddWorkstationButton },
  data() {
    return {
      // 初始任务池数据
      tasks: [
        { id: 1, name: "任务A", hours: 6 },
        { id: 2, name: "任务B", hours: 4 },
        { id: 3, name: "任务C", hours: 5 },
      ],
      // 初始工位数据
      workstations: [
        { id: 1, name: "工位1", tasks: [] },
      ],
    };
  },
  methods: {
    // 添加新工位
    addWorkstation(name) {
      this.workstations.push({
        id: this.workstations.length + 1,
        name,
        tasks: [],
      });
    },

    // 添加任务到任务池
    addTask(task) {
      this.tasks.push(task);
    },

    // 从任务池删除任务
    deleteTask(taskId) {
      this.tasks = this.tasks.filter((task) => task.id !== taskId);
    },

    // 将任务分配到工位
    assignTaskToWorkstation({ task, workstationId, hour }) {
      const workstation = this.workstations.find((ws) => ws.id === workstationId);
      const endHour = hour + task.hours;

      // 检查该时间段是否有任务重叠
      const hasOverlap = workstation.tasks.some(
        (t) => (t.start < endHour && t.start >= hour)
      );
      
      if (!hasOverlap) {
        // 从任务池移除任务
        this.tasks = this.tasks.filter((t) => t.id !== task.id);

        // 将任务添加到工位的任务列表中
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

/* 左侧面板样式 */
.left-panel {
  width: 20%;
  padding: 10px;
  border-right: 1px solid #ccc;
}

/* 右侧面板样式 */
.right-panel {
  width: 80%;
  padding: 10px;
}
</style>
