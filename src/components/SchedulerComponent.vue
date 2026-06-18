<script setup>
import { onMounted, onUnmounted, ref } from "vue";
import { Scheduler } from "@dhx/trial-scheduler";

const props = defineProps({
  events: {
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits(["event-updated"]);

const schedulerContainer = ref(null);
const schedulerInstance = ref(null);

onMounted(() => {
  const scheduler = Scheduler.getSchedulerInstance();
  schedulerInstance.value = scheduler;

  scheduler.config.header = [
    "day",
    "week",
    "month",
    "date",
    "prev",
    "today",
    "next",
  ];

  scheduler.init(schedulerContainer.value, new Date(2027, 5, 10), "week");
  scheduler.parse(props.events);

  scheduler.createDataProcessor((entity, action, data, id) => {
    if (entity === "event") {
      emit("event-updated", id, action, data);
    }
  });
});

onUnmounted(() => {
  schedulerInstance.value?.destructor();
  schedulerInstance.value = null;

  if (schedulerContainer.value) {
    schedulerContainer.value.innerHTML = "";
  }
});
</script>

<template>
  <div ref="schedulerContainer"></div>
</template>

<style>
  @import "@dhx/trial-scheduler/codebase/dhtmlxscheduler.css";
</style>