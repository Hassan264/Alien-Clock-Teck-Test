<!-- AlienClock.vue -->
<template>
    <div>
      <h2>Alien Clock</h2>
      <div>{{ alienTime }}</div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AlienClock',
    data() {
      return {
        alienTime: ''
      };
    },
    created() {
      this.updateAlienClock();
    },
    methods: {
      updateAlienClock() {
        setInterval(() => {
          const earthTime = new Date(); // Get current Earth time
          const alienTime = this.calculateAlienTime(earthTime); // Calculate Alien time
          this.alienTime = this.formatAlienTime(alienTime); // Format Alien time for display
        }, 1000); // Update every 1 second
      },
      calculateAlienTime(earthTime) {
        const earthTimeInSeconds = earthTime.getTime() / 1000; // Convert Earth time to seconds
        const alienTimeInSeconds = earthTimeInSeconds * 2; // 1 second Alien time is 0.5 second Earth time
        return new Date(alienTimeInSeconds * 1000); // Convert seconds back to milliseconds
      },
      formatAlienTime(alienTime) {
        const hours = alienTime.getUTCHours().toString().padStart(2, '0');
        const minutes = alienTime.getUTCMinutes().toString().padStart(2, '0');
        const seconds = alienTime.getUTCSeconds().toString().padStart(2, '0');
        return `${hours}:${minutes}:${seconds}`;
      }
    }
  }
  </script>
  