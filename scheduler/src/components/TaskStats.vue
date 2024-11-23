<template>
    <div class="task-stats">
      <h3>统计信息</h3>
      <div>
        <p>已分配任务数: {{ assignedTaskCount }}</p>
        <p>待分配任务数: {{ unassignedTaskCount }}</p>
        <p>工位利用率: {{ workstationUtilization }}%</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "TaskStats",
    props: {
      tasks: Array, // 全部任务
      workstations: Array, // 全部工位
    },
    computed: {
      // 已分配任务数
      assignedTaskCount() {
        return this.workstations.reduce((count, ws) => count + ws.tasks.length, 0);
      },
      // 待分配任务数
      unassignedTaskCount() {
        return this.tasks.length;
      },
      // 工位占用率
      workstationUtilization() {
        const totalTime = 10 * this.workstations.length; // 每个工位的总小时数
        const usedTime = this.workstations.reduce(
          (sum, ws) => sum + ws.tasks.reduce((tSum, task) => tSum + task.hours, 0),
          0
        );
        return totalTime === 0 ? 0 : ((usedTime / totalTime) * 100).toFixed(1);
      },
    },
  };
  </script>
  
  <style scoped>
  .task-stats {
    margin-top: 20px;
    border: 1px solid #ccc;
    padding: 10px;
    background-color: #f9f9f9;
    border-radius: 5px;
  }
  h3 {
    margin-bottom: 10px;
    font-size: 1.2em;
  }
  p {
    margin: 5px 0;
  }
  </style>
  