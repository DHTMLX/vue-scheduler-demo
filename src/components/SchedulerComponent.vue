
<template>
  <div ref="SchedulerComponent"></div>
</template>

<script>
import "dhtmlx-scheduler";

/*global scheduler*/

export default {
  props: {
    events: {
      type: Array,
      default() {
        return []
      },
    },
  },
  methods: {
    $_initSchedulerEvents: function () {
      if (!scheduler.$_eventsInitialized) {
        scheduler.attachEvent("onEventAdded", (id, ev) => {
          this.$emit("event-updated", id, "inserted", ev);
        });
        scheduler.attachEvent("onEventChanged", (id, ev) => {
          this.$emit("event-updated", id, "updated", ev);
        });
        scheduler.attachEvent("onEventDeleted", (id) => {
          this.$emit("event-updated", id, "deleted");
        });

        scheduler.$_eventsInitialized = true;
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
    this.$_initSchedulerEvents();
    
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