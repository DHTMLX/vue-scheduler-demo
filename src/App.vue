<script setup>
import { ref } from "vue";
import SchedulerComponent from "./components/SchedulerComponent.vue";

const events = ref([
  { id: 1, start_date: "2027-06-10 6:00", end_date: "2027-06-10 8:00", text: "Event 1" },
  { id: 2, start_date: "2027-06-13 10:00", end_date: "2027-06-13 18:00", text: "Event 2" },
]);

const messages = ref([]);
let nextMessageId = 1;

function addMessage(text) {
  messages.value.unshift({
    id: nextMessageId,
    text,
  });
  nextMessageId += 1;

  if (messages.value.length > 40) {
    messages.value.pop();
  }
}

function logEventUpdate(id, mode, event) {
  const eventText = event?.text ? ` (${event.text})` : "";
  addMessage(`Event ${mode}: ${id} ${eventText}`);
}
</script>

<template>
  <div class="container">
    <SchedulerComponent class="left-container" :events="events" @event-updated="logEventUpdate" />
    <div class="right-container">
      <ul class="scheduler-messages">
        <li class="scheduler-message" :key="message.id" v-for="message in messages">
          {{ message.text }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style>
  html, body {
    height: 100%;
    margin: 0;
    padding: 0;
  }

  .container {
    display: flex;
    height: 100%;
    width: 100%;
  }

  .left-container {
    overflow: hidden;
    position: relative;
    height: 100vh;
    width: 80vw;
  }

  .right-container {
    border-right: 1px solid #cecece;
    float: right;
    height: 100%;
    width: 340px;
    box-shadow: 0 0 5px 2px #aaa;
    position: relative;
    z-index:2;
  }

.scheduler-messages {
    list-style-type: none;
    height: 50%;
    margin: 0;
    overflow-x: hidden;
    overflow-y: auto;
    padding-left: 5px;
  }

  .scheduler-messages > .scheduler-message {
    background-color: #f4f4f4;
    box-shadow:inset 5px 0 #d69000;
    font-family: Geneva, Arial, Helvetica, sans-serif;
    font-size: 14px;
    margin: 5px 0;
    padding: 8px 0 8px 10px;
  }
</style>