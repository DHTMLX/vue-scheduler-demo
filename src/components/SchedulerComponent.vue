<template>
  <div ref="SchedulerComponent"></div>
</template>

<script>
import { Scheduler } from "@dhx/trial-scheduler";

export default {
  props: {
    events: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  
  mounted() {
    let scheduler = Scheduler.getSchedulerInstance();
    this.scheduler = scheduler;
    scheduler.skin = "material";
    scheduler.config.header = [
      "day",
      "week",
      "month",
      "date",
      "prev",
      "today",
      "next",
    ];

    scheduler.init(
      this.$refs.SchedulerComponent,
      new Date(2024, 0, 20),
      "week"
    );
    scheduler.parse(this.$props.events);

    scheduler.createDataProcessor((entity, action, data, id) => { 
      this.$emit(`${entity}-updated`, id, action, data);
    });
  },
  unmounted() {
    this.scheduler.destructor();
  },
};
</script>
 
<style>
  @import "@dhx/trial-scheduler/codebase/dhtmlxscheduler.css";
</style>