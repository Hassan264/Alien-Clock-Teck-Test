<!-- EarthDateTime.vue -->
<template>
    <div>
      <h2>Earth Date and Time</h2>
      <div>{{ earthTime }}</div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'EarthDateTime',
    props: {
      alienTime: {
        type: Date,
        required: true
      }
    },
    computed: {
      earthTime() {
        const earthTimeInSeconds = this.alienTime.getTime() / 1000 / 2; // Convert Alien time to Earth time
        return new Date(earthTimeInSeconds * 1000); // Convert seconds back to milliseconds
      }
    }
  }
  </script>
  