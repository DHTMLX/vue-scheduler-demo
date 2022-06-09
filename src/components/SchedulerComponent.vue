
<template>
  <div ref="SchedulerComponent"></div>
</template>

<script>
import { scheduler } from "dhtmlx-scheduler";

export default {
  props: {
    events: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  methods: {    
    $_initDataProcessor: function() {
      if (!scheduler.$_dataProcessorInitialized) {
        scheduler.createDataProcessor((entity, action, data, id) => { 
          this.$emit(`${entity}-updated`, id, action, data);
        });
        scheduler.$_dataProcessorInitialized = true;
      }
    },
  
  },
  mounted: function () {
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
    this.$_initDataProcessor();

    scheduler.init(
      this.$refs.SchedulerComponent,
      new Date(2020, 0, 20),
      "week"
    );
    scheduler.parse(this.$props.events);
  },
};
</script>

<style>
@import "~dhtmlx-scheduler/codebase/dhtmlxscheduler_material.css";
</style>