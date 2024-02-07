<!-- Alarm.vue -->
<template>
    <div>
      <h2>Set Alarm</h2>
      <form @submit.prevent="setAlarm">
        <input type="datetime-local" v-model="alarmTime" required>
        <button type="submit">Set Alarm</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Alarm',
    data() {
      return {
        alarmTime: ''
      };
    },
    methods: {
      setAlarm() {
        const alarmTime = new Date(this.alarmTime);
        this.$emit('set-alarm', alarmTime);
      }
    }
  }
  </script>
  