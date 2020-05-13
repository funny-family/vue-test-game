<template>
  <div id="app">
    <h1>Let's play a numeric game!</h1>
    <Message
      :message="message"
      v-if="isStateAnswer"
    />
    <Counter
      amount="5"
      :timeout-handler="timeoutHandler"
      v-if="isStateRun"
    />
    <div
      :style="{
        color:
          gameResultMessage === 'Correct!'? '#2CE855' :
          gameResultMessage === 'Time is over!' ? '#2c3e50' :
          '#EB3D3D'
      }"
    >
      {{ gameResultMessage }}
    </div>
    <Question
      :question="question"
      v-if="isStateRun"
    />
    <AnswerForm
      :handler="answerHandler"
      v-if="isStateRun"
    />
    <button
      @click="startClicked"
      v-if="isStateIdle || isStateAnswer"
    >
      Start game!
    </button>
  </div>
</template>

<script>
import Question from './components/Question.vue';
import Counter from './components/Counter.vue';
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
    Counter,
    AnswerForm,
    Message
  },
  data: () => ({
    gameState: GAME_STATE_IDLE,
    question: null,
    userAnswer: '',
    gameResultMessage: ''
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
    timeoutHandler() {
      this.$data.gameResultMessage = 'Time is over!';
      this.$data.gameState = GAME_STATE_IDLE;
    },
    answerHandler(answer) {
      if (checkAnswer(this.$data.question, Number.parseInt(answer))) {
        this.$data.gameResultMessage = 'Correct!';
      } else {
        this.$data.gameResultMessage = 'Wrong!';
      }
      // TODO?
      //this.$data.gameState = GAME_STATE_SHOW_ANSWER;
    },
    startClicked() {
      this.$data.question = generateQuestion();
      this.$data.gameState = GAME_STATE_RUN;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
