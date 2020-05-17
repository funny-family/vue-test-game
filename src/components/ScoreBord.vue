<template>
  <div
    class="scoreboard-container"
    :style="{
      opacity:
        isScoreboardVisible === true ? '1' :
        '0'
    }"
  >
    <div class="current-score">
      Score: {{ currentScrore }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currentScrore: {
      type: Number,
      default: 0
    },
    isScoreboardVisible: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss" scoped>
.scoreboard-container {
  padding-top: 40px;
  padding-left: 60px;
  position: absolute;
}
.current-score {
  font-size: 28px;
  font-weight: bolder;
}
</style>