<template>
  <div id="app">
    <b-alert :show="isReseting" variant="secondary" class="quiz-alert">
      Reseting Quiz... ⏳
    </b-alert>

    <b-alert :show="showSuccessfulReset" variant="success" class="quiz-alert">
      Quiz Reset Successful ✔
    </b-alert>

    <b-container class="bv-example-row">
      <b-row>
        <b-col
          xl="6"
          lg="8"
          md="10"
          sm="12"
          offset-xl="3"
          offset-lg="2"
          offset-md="1"
        >
          <Header
            :num-correct-answers="numCorrectAnswers"
            :num-questions="questions.length"
          />

          <QuestionBox
            v-if="questions.length"
            :key="currentQuestionIndex"
            :is-reseting="isReseting"
            :current-question="questions[currentQuestionIndex]"
            :next="next"
            :update-score="updateScore"
          />
        </b-col>
      </b-row>
    </b-container>

    <div>
      <b-button :disabled="isReseting" @click="reset">
        Reset Quiz
      </b-button>
    </div>

    <Footer />
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import QuestionBox from './components/QuestionBox.vue';
import Footer from './components/Footer.vue';
const DATA_URI =
  'https://opentdb.com/api.php?amount=10&category=27&type=multiple';

export default {
  name: 'App',

  components: {
    Header,
    QuestionBox,
    Footer
  },

  data() {
    return {
      questions: [],
      currentQuestionIndex: 0,
      numCorrectAnswers: 0,
      isReseting: false,
      showSuccessfulReset: false
    };
  },

  async mounted() {
    const response = await axios.get(DATA_URI);
    this.questions = response.data.results;
  },

  methods: {
    next() {
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex += 1;
      }
    },

    updateScore(isCorrectAnswer) {
      if (isCorrectAnswer) {
        this.numCorrectAnswers += 1;
      }
    },

    async reset() {
      this.isReseting = true;

      this.currentQuestionIndex = 0;
      this.numCorrectAnswers = 0;
      const response = await axios.get(DATA_URI);
      this.questions = response.data.results;

      this.isReseting = false;
      this.showSuccessfulReset = true;
      setTimeout(() => {
        this.showSuccessfulReset = false;
      }, 1000);
    }
  }
};
</script>

<style>
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale; */
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  /* margin-bottom: 20px; */

  background: linear-gradient(
    to right,
    #396afc,
    #2948ff
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.quiz-alert {
  position: fixed !important;
  top: 0;
  width: 100%;
}

.ma-bot-sm {
  margin-bottom: 20px;
}
</style>
