<!-- SetDateTimeForm.vue -->
<template>
    <div>
      <h2>Set Date and Time</h2>
      <form @submit.prevent="setEarthDateTime">
        <input type="datetime-local" v-model="earthDateTime" required>
        <button type="submit">Set</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'SetDateTimeForm',
    data() {
      return {
        earthDateTime: ''
      };
    },
    methods: {
      setEarthDateTime() {
        const earthTime = new Date(this.earthDateTime);
        this.$emit('set-earth-time', earthTime);
      }
    }
  }
  </script>
  