<template>
  <div class="timer">
    <h1>Pomodoro Timer</h1>
    <div class="time-display">
      <p>{{ minutes }}:{{ seconds < 10 ? '0' + seconds : seconds }}</p>
    </div>
    <div class="status">
      <p>{{ isBreak ? 'Break time' : 'Time to work' }}</p>
    </div>
    <div class="controls">
      <button @click="startTimer" :disabled="isRunning">Start</button>
      <button @click="pauseTimer">Pause</button>
      <button @click="resetTimer">Reset</button>
    </div>
    <div class="cycle-count">
      <p>Completed cycles: {{ cycleCount }}</p>
      <p>Total working time: {{ totalWorkTimeHours }} hours {{ totalWorkTimeMinutes }} minutes</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      workTime: 1500, 
      breakTime: 300, 
      time: 1500,
      isBreak: false, 
      timerInterval: null,
      isRunning: false,
      cycleCount: 0,
      totalWorkTime: 0,
    };
  },
  computed: {
    minutes() {
      return Math.floor(this.time / 60);
    },
    seconds() {
      return this.time % 60;
    },
    totalWorkTimeHours() {
      return Math.floor(this.totalWorkTime / 3600);
    },
    totalWorkTimeMinutes() {
      return Math.floor((this.totalWorkTime % 3600) / 60);
    },
  },
  methods: {
    startTimer() {
      if (!this.isRunning) {
        this.isRunning = true;
        this.timerInterval = setInterval(() => {
          if (this.time > 0) {
            this.time--;
          } else {
            this.switchTimer();
          }
        }, 1000);
      }
    },
    pauseTimer() {
      clearInterval(this.timerInterval);
      this.isRunning = false;
    },
    resetTimer() {
      clearInterval(this.timerInterval);
      this.time = this.workTime;
      this.isBreak = false;
      this.isRunning = false;
      this.cycleCount = 0;
      this.totalWorkTime = 0;
    },
    switchTimer() {
      clearInterval(this.timerInterval);
      this.isRunning = false;
      if (this.isBreak) {
        this.time = this.workTime;
        this.isBreak = false;
        this.cycleCount++;
      } else {
        this.totalWorkTime += this.workTime;
        this.time = this.breakTime;
        this.isBreak = true;
      }
      this.startTimer();
    },
  },
};
</script>

<style scoped>
.timer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 98vh;
  background-color: #484848;
}

h1 {
  font-size: 50px;
  margin-bottom: 20px;
}

.time-display {
  font-size: 80px;
  margin-bottom: 20px;
}

.status {
  font-size: 30px;
  margin-bottom: 20px;
}

.controls {
  display: flex;
  gap: 10px;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  background-color: rgb(183, 0, 0);
  color: #c3c3c3;
  border: none;
  border-radius: 5px;
}

button:disabled {
  background-color: #969696;
  cursor: not-allowed;
}

.cycle-count {
  margin-top: 20px;
  font-size: 25px;
}
</style>
