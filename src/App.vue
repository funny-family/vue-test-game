<template>
  <div id="app">
    <div class="app-container">
      <StartModalWindow
        :show="isModalVisible"
        @close="startClicked"
      />
      <Message
        :message="answerResultMessage"
        v-if="isStateAnswer"
      />
      <Timer
        :resetGameTime="resetGameTime"
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
      <GameStartTimer
        :timeToStart="5"
        :startGameAfterTimerEnd="startGameAfterTimerEnd"
        v-if="isGameStating"
      />
      <!-- <div class="game-start-button">
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
      </div> -->
    </div>
  </div>
</template>

<script>
import Question from './components/Question.vue';
import Timer from './components/Timer';
import GameStartTimer from './components/GameStartTimer';
import AnswerForm from './components/AnswerForm.vue';
import Message from './components/Message.vue';
import StartModalWindow from './components/StartModalWindow';
import { checkAnswer, generateQuestion } from './services/questions';

const GAME_STATE_IDLE = 'idle';
const GAME_STATE_RUN = 'run';
const GAME_STATE_SHOW_ANSWER = 'showAnswer';

export default {
  name: 'App',
  components: {
    StartModalWindow,
    Question,
    Timer,
    GameStartTimer,
    AnswerForm,
    Message
  },
  data: () => ({
    gameState: GAME_STATE_IDLE,
    isModalVisible: true,
    isGameStating: false,
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
    startGameAfterTimerEnd() {
      this.$data.gameState = GAME_STATE_RUN;
      this.$data.answerResultMessage = '';
      this.$data.question = generateQuestion();
      this.$data.isGameStating = false;
    },
    startClicked() {
      this.$data.isModalVisible = false;
      this.$data.isGameStating = true;
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
    close() {
      this.$emit('close');
    }
  }
};
</script>

<style lang="scss">
@import './components/globalButtonStyle';
@import './components/colorStyles';

.app-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.game-start-button {
  display: flex;
  justify-content: center;
  align-items: center;
}

#app {
  font-family: 'Jost';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: $Bluewood;
  padding: 0;
  margin: 0;
}

html {
  width: 100%;
  height: calc(var(--vh, 1vh) * 100);
}

body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}
</style>
