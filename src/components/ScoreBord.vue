<template>
  <div
    class="scoreboard-container"
    :style="{
      opacity:
        show === true ? '1' :
        '0'
    }"
  >
    <div class="current-score score-style">
      Score: {{ currentScrore }}
    </div>
    <div class="first-place score-style">
      Top 1: {{ scoresContainer[0] }}
    </div>
    <div class="second-place score-style">
      Top 2: {{ scoresContainer[1] }}
    </div>
    <div class="third-place score-style">
      Top 3: {{ scoresContainer[2] }}
    </div>
  </div>
</template>

<script>
// let scoreContainer = [].splice(0, 3).sort((a, b) => b - a);

export default {
  props: {
    currentScrore: {
      type: Number,
      default: 0
    },
    scoresContainer: {
      type: Array,
      default: () => [].splice(0, 3).sort((a, b) => b - a)
    },
    show: {
      type: Boolean,
      default: true
    }
  },
  data: () => ({
    firstPlace: 0,
    secondPlace: 0,
    thirdPlace: 0
  }),
  //
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
}

.first-place {
  font-size: 20px;
}

.second-place {
  font-size: 20px;
}

.third-place {
  font-size: 20px;
}

.score-style {
  font-weight: bolder;
}
</style>