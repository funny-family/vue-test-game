<template>
  <div id="app">
    <h1>Let's play a numeric game!</h1>
    <Message
      :message="answerResultMessage"
      v-if="isStateAnswer"
    />
    <Timer
      :reset-game-time="resetGameTime"
      :timeLimit="timerTimeLimit"
      v-model="testNum"
      v-if="isStateRun"
    />
    <div
      :style="{
        color:
          answerResultMessage === 'Correct!'? '#2CE855' :
          answerResultMessage === 'Time is over!' ? '#2c3e50' :
          '#EB3D3D'
      }"
    >
      {{ answerResultMessage }}
    </div>
    <Question
      :question="question"
      v-if="isStateRun"
    />
    <AnswerForm
      :handler="answerHandler"
      v-model="answerResult"
      v-if="isStateRun"
    />
    <button
      @click="startClicked"
      v-if="isStateIdle || isStateAnswer"
    >
      <div v-if="answerResultMessage === 'Time is over!'">
        Restart game
      </div>
      <div v-else>
        Start game!
      </div>
    </button>
  </div>
</template>

<script>
import Question from './components/Question.vue';
import Timer from './components/Timer';
import AnswerForm from './components/AnswerForm.vue';
import Message from './components/Message.vue';
import { checkAnswer, generateQuestion } from './services/questions';

const GAME_STATE_IDLE = 'idle';
const GAME_STATE_RUN = 'run';
const GAME_STATE_SHOW_ANSWER = 'showAnswer';

export default {
  name: 'App',
  components: {
    Question,
    Timer,
    AnswerForm,
    Message
  },
  data: () => ({
    gameState: GAME_STATE_IDLE,
    question: null,
    userAnswer: '',
    answerResultMessage: '',
    answerResult: '',
    timerTimeLimit: 100,
    testNum: Number,
    initialTime: 100
  }),
  computed: {
    isStateRun() {
      return this.$data.gameState === GAME_STATE_RUN;
    },
    isStateAnswer() {
      return this.$data.gameState === GAME_STATE_SHOW_ANSWER;
    },
    isStateIdle() {
      return this.$data.gameState === GAME_STATE_IDLE;
    }
  },
  methods: {
    resetGameTime() { // stop game when time go beyond 0
      this.$data.answerResultMessage = 'Time is over!';
      this.$data.timerTimeLimit = 100;
      this.$data.gameState = GAME_STATE_IDLE;
    },
    answerHandler(answer) {
      if (checkAnswer(this.$data.question, Number.parseInt(answer))) {
        this.$data.answerResultMessage = 'Correct!';
        this.$data.timerTimeLimit += 4;
        this.$data.answerResult = '';
        setTimeout(() => {
          this.$data.answerResultMessage = ''
        }, 500);
        this.$data.question = generateQuestion();
      } else {
        this.$data.answerResultMessage = 'Wrong!';
      }
      // TODO?
      // this.$data.gameState = GAME_STATE_SHOW_ANSWER;
    },
    startClicked() {
      this.$data.question = generateQuestion();
      this.$data.gameState = GAME_STATE_RUN;
      this.$data.answerResultMessage = '';
    }
  }
};
</script>

<style lang="scss">
@import './components/globalButtonStyle';

#app {
  font-family: 'Jost';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
