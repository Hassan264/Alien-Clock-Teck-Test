<template>
  <div class="container">
    <div class="alien-clock">
      <h2>Alien Clock</h2>
      <div class="time-display">
        <p class="alien-time">Current Alien Time: {{ alienTime }}</p>
        <p class="earth-time">Current Earth Time: {{ earthTime }}</p>
      </div>
      <button class="set-alarm-btn" @click="openSetAlarmModal">Set Alarm</button>
      <button class="reset-btn" @click="reset">Reset</button>
    </div>
    <div class="set-time">
      <h3>Set Time</h3>
      <div class="input-fields">
        <div class="input-field">
          <label for="year">Year:</label>
          <input type="number" v-model="inputYear" placeholder="Year">
        </div>
        <div class="input-field">
          <label for="month">Month:</label>
          <input type="number" v-model="inputMonth" placeholder="Month">
        </div>
        <div class="input-field">
          <label for="day">Day:</label>
          <input type="number" v-model="inputDay" placeholder="Day">
        </div>
        <div class="input-field">
          <label for="hour">Hour:</label>
          <input type="number" v-model="inputHour" placeholder="Hour">
        </div>
        <div class="input-field">
          <label for="minute">Minute:</label>
          <input type="number" v-model="inputMinute" placeholder="Minute">
        </div>
        <div class="input-field">
          <label for="second">Second:</label>
          <input type="number" v-model="inputSecond" placeholder="Second">
        </div>
      </div>
      <button class="set-time-btn" @click="setAlienTime">Set Alien Time</button>
    </div>

    <!-- Alarm Modal -->
    <div v-if="showAlarmModal" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <h2>Set Alarm</h2>
        <div class="input-fields">
          <!-- Input fields for setting the alarm time -->
          <div class="input-field">
            <label for="alarm-year">Year:</label>
            <input type="number" id="alarm-year" v-model="alarmYear" placeholder="Year">
          </div>
          <div class="input-field">
            <label for="alarm-month">Month:</label>
            <input type="number" id="alarm-month" v-model="alarmMonth" placeholder="Month">
          </div>
          <div class="input-field">
            <label for="alarm-day">Day:</label>
            <input type="number" id="alarm-day" v-model="alarmDay" placeholder="Day">
          </div>
          <div class="input-field">
            <label for="alarm-hour">Hour:</label>
            <input type="number" id="alarm-hour" v-model="alarmHour" placeholder="Hour">
          </div>
          <div class="input-field">
            <label for="alarm-minute">Minute:</label>
            <input type="number" id="alarm-minute" v-model="alarmMinute" placeholder="Minute">
          </div>
          <div class="input-field">
            <label for="alarm-second">Second:</label>
            <input type="number" id="alarm-second" v-model="alarmSecond" placeholder="Second">
          </div>
        </div>
        <button class="set-alarm-btn" @click="setAlarm">Set Alarm</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      year: 2803,
      month: 18,
      day: 31,
      hour: 2,
      minute: 2,
      second: 88,
      earthTime: "",
      alienTime: "",
      inputYear: 2804,
      inputMonth: 18,
      inputDay: 31,
      inputHour: 2,
      inputMinute: 2,
      inputSecond: 88,
      intervalId: null,
      showAlarmModal: false,
      alarmYear: 2804,
      alarmMonth: 1,
      alarmDay: 1,
      alarmHour: 0,
      alarmMinute: 0,
      alarmSecond: 0
    };
  },

  mounted() {
    this.updateClock();
    this.intervalId = setInterval(this.updateClock, 500);
  },
  destroyed() {
    clearInterval(this.intervalId);
  },
  methods: {
    updateClock() {
      // Update Alien Time
      this.second++;
      if (this.second >= 90) {
        this.second = 0;
        this.minute++;
      }
      if (this.minute >= 90) {
        this.minute = 0;
        this.hour++;
      }
      if (this.hour >= 36) {
        this.hour = 0;
        this.day++;
      }
      if (this.day > 18) {
        this.day = 1;
        this.month = 1;
        this.year++;
      }
      this.alienTime = `${this.year}/${this.month}/${this.day},  ${this.hour}:${this.minute}:${this.second}`;



      // Update Earth time
      const daysInMonths = [44, 42, 48, 40, 48, 44, 40, 44, 42, 40, 40, 42, 44, 48, 42, 40, 44, 38];
      const secondsInMinute = this.minute * 90;
      const secondsInHour = this.hour * 90 * 90;
      const secondsInDay = this.day * 36 * 90 * 90;
      const secondsInMonth = this.daysInMonth(this.month - 1)
      const secondsInYear = daysInMonths.reduce((acc, days) => acc + (secondsInDay * days), 0)
      const totalSeconds = this.second + secondsInMinute + secondsInHour + secondsInDay + secondsInMonth + (this.year - 2804) * secondsInYear;
      const earthSecond = totalSeconds * 0.5;
      const earthDate = new Date(1970, 0, 1, 0, 0, 0);
      earthDate.setSeconds(earthDate.getSeconds() + earthSecond);
      this.earthTime = earthDate.toLocaleString();

      // Check for alarm
      this.checkAlarm();
    },
    setAlienTime() {
      // Validate input
      const dayInTheMonth = this.daysInMonth(this.inputMonth)
      if (
        this.inputYear < 2804 ||
        this.inputMonth < 1 ||
        this.inputMonth > 18 ||
        this.inputDay < 1 ||
        this.inputDay > dayInTheMonth ||
        this.inputHour < 0 ||
        this.inputHour > 35 ||
        this.inputMinute < 0 ||
        this.inputMinute > 89 ||
        this.inputSecond < 0 ||
        this.inputSecond > 89
      ) {
        alert("Invalid Alien Time");
        return;
      }

      this.year = this.inputYear
      this.month = this.inputMonth
      this.day = this.inputDay
      this.hour = this.inputHour
      this.minute = this.inputMinute
      this.second = this.inputSecond

      this.alienTime = `Year ${this.inputYear}, Month ${this.inputMonth}, Day ${this.inputDay}, Hour ${this.inputHour}, Minute ${this.inputMinute}, Second ${this.inputSecond}`;

      // Update Earth Time
      const daysInMonths = [44, 42, 48, 40, 48, 44, 40, 44, 42, 40, 40, 42, 44, 48, 42, 40, 44, 38];
      const secondsInMinute = this.inputMinute * 90;
      const secondsInHour = this.inputHour * 90 * 90;
      const secondsInDay = this.inputDay * 36 * 90 * 90;
      const secondsInMonth = this.daysInMonth(this.inputMonth - 1)
      const secondsInYear = daysInMonths.reduce((acc, days) => acc + (secondsInDay * days), 0);
      const totalSeconds = this.inputSecond + secondsInMinute + secondsInHour + secondsInDay + secondsInMonth + (this.inputYear - 2804) * secondsInYear;
      const earthSecond = totalSeconds * 0.5;
      const earthDate = new Date(1970, 0, 1, 0, 0, 0);
      earthDate.setSeconds(earthDate.getSeconds() + earthSecond);
      this.earthTime = earthDate.toLocaleString();
    },
    daysInMonth(month) {
      // Define days in each month
      const daysInMonths = [44, 42, 48, 40, 48, 44, 40, 44, 42, 40, 40, 42, 44, 48, 42, 40, 44, 38];
      return month > 0 ? daysInMonths[month - 1] : daysInMonths[month]; // Adjust for array index
    },
    openSetAlarmModal() {
      this.showAlarmModal = true;
    },
    closeModal() {
      this.showAlarmModal = false;
    },
    setAlarm() {
      this.closeModal();
      alert("Alarm is set!");
    },
    checkAlarm() {
      const currentAlienTime = `${this.year}:${this.month}:${this.day}:${this.hour}:${this.minute}:${this.second}`;
      const alarmTime = `${this.alarmYear}:${this.alarmMonth}:${this.alarmDay}:${this.alarmHour}:${this.alarmMinute}:${this.alarmSecond}`;

      if (currentAlienTime === alarmTime) {
        this.triggerAlarm();
      }
    },
    triggerAlarm() {
      // Implement alarm triggering action
      alert("Alarm!");
    },
    reset() {
      // Reset to default values
      this.year = this.inputYear = 2803
      this.month = this.inputMonth = 18
      this.day = this.inputDay = 31
      this.hour = this.inputHour = 2
      this.minute = this.inputMinute = 2
      this.second = this.inputSecond = 88
      this.alienTime = "";
      this.earthTime = "";
    }
  }
};
</script>

<style scoped>
/* Styles for the modal */
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: #fefefe;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.alien-clock {
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.time-display {
  margin-bottom: 20px;
}

.alien-time {
  font-size: 18px;
  color: #333;
}

.earth-time {
  font-size: 18px;
  color: #333;
}

.reset-btn,
.set-alarm-btn {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  margin: 5px;
  transition: background-color 0.3s;
}

.set-alarm-btn:hover,
.reset-btn:hover,
.set-time-btn:hover {
  background-color: #0056b3;
}

.set-time {
  margin-top: 20px;
}

.set-time h3 {
  font-size: 20px;
  margin-bottom: 10px;
}

.input-fields {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.input-field {
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
}

.input-field label {
  margin: 0 5px;
  padding-bottom: 10px;
}

.input-field input {
  width: 50px;
  padding: 8px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.input-fields input {
  width: 70px;
  padding: 8px;
  margin: 0 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.set-time-btn {
  background-color: #28a745;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.set-time-btn:hover {
  background-color: #218838;
}
</style>