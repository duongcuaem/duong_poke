<template>
    <div class="screenTime">
      <p>{{ minutes }} phút {{ seconds }} giây</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        minutes: 0,
        seconds: 0,
        timer: null
      };
    },
    methods: {
      startTimer() {
        this.timer = setInterval(() => {
          if (this.seconds < 59) {
            this.seconds++;
          } else {
            this.seconds = 0;
            this.minutes++;
          }
        }, 1000); // Mỗi 1000ms (1 giây)
      },
      stopTimer() {
        clearInterval(this.timer);
      },
      resetTimer() {
        this.minutes = 0;
        this.seconds = 0;
      }
    },
    created() {
      this.startTimer();
    },
    beforeDestroy() {
      this.stopTimer();
    }
  };
  </script>
  <style lang="css" scoped>

  .screenTime{
    margin-left: 20px;
  }

  </style>
  