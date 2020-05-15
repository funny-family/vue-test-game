<template>
  <div>
    {{ time }}
  </div>
</template>

<script>
const timerTickDelay = 1000;

export default {
  props: {
    timeToStart: {
      type: Number
    },
    startGameAfterTimerEnd: {
      type: Function
    }
  },
  data: () => ({
    time: 0
  }),
  created() {
    this.$data.time = this.$props.timeToStart;
    this.timer = setTimeout(this.doTimerTick, timerTickDelay);
  },
  destroyed() {
    clearTimeout(this.timer);
  },
  methods: {
    doTimerTick() {
      this.$data.time -= timerTickDelay / 1000;
      if (this.$data.time > 0) {
        this.timer = setTimeout(this.doTimerTick, timerTickDelay);
      } else {
        this.$props.startGameAfterTimerEnd();
      }
    }
  }
};
</script>
