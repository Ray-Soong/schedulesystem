<template>
    <div class="workstation-pool">
      <h3>工位池</h3>
      <FullCalendar
        :plugins="[timeGridPlugin, interactionPlugin]" 
        initialView="timeGridWeek"
        :slotMinTime="'08:00:00'"
        :slotMaxTime="'18:00:00'"
        @eventReceive="onTaskDrop"
        ref="calendar"
      />
    </div>
  </template>
  
  <script>
  import FullCalendar from "@fullcalendar/vue3";
  
  export default {
    name: "WorkstationPool",
    components: { FullCalendar },
    methods: {
      onTaskDrop(info) {
        const task = JSON.parse(info.draggedEl.dataset.task);
        const calendarApi = this.$refs.calendar.getApi();
        calendarApi.addEvent({
          title: task.name,
          start: info.date,
          duration: { hours: task.hours },
        });
      },
    },
  };
  </script>
  
  <style>
  .workstation-pool {
    border: 1px solid #ccc;
    padding: 10px;
  }
  </style>
  