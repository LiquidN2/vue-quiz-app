<template>
  <div class="question-box-conatainer">
    <b-jumbotron>
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>
        <!-- {{ question.question }} -->
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          :data-answer="answer"
          :active="selected === answer"
          :class="{
            correct: submitted && answer === currentQuestion.correct_answer,
            incorrect:
              submitted &&
              answer !== currentQuestion.correct_answer &&
              selected === answer
          }"
          @click="select"
          v-html="answer"
        />
      </b-list-group>

      <b-button
        variant="primary"
        :disabled="!selected || submitted || isReseting"
        @click="submit"
      >
        Submit
      </b-button>

      <b-button variant="success" :disabled="isReseting" @click="next">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import shuffle from 'lodash/shuffle';

export default {
  props: {
    isReseting: {
      type: Boolean,
      default: false
    },

    currentQuestion: {
      type: Object,
      default() {
        return {
          question: 'Getting data ...',
          correct_answer: 'Getting data ...',
          incorrect_answers: []
        };
      }
    },

    next: {
      type: Function,
      default() {
        return () => {};
      }
    },

    updateScore: {
      type: Function,
      default() {
        return () => {};
      }
    }
  },

  data() {
    return {
      selected: null,
      submitted: false,
      classObject: {
        correct: false
      }
    };
  },

  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      return shuffle(answers);
    }
  },

  methods: {
    select(event) {
      this.selected = event.target.dataset.answer;
    },

    submit() {
      let isCorrectAnswer = false;

      if (this.selected === this.currentQuestion.correct_answer) {
        isCorrectAnswer = true;
        this.classObject.correct = true;
      }

      if (!this.submitted) {
        this.updateScore(isCorrectAnswer);
        this.submitted = true;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.list-group {
  margin-bottom: 25px;

  &-item {
    cursor: pointer;

    &:hover {
      background-color: #eee;
    }

    &:hover.active {
      background-color: #007bff !important;
      border-color: #007bff !important;
    }
  }
}

.correct {
  background-color: LimeGreen !important;
  border-color: LimeGreen !important;
}

.incorrect {
  background-color: OrangeRed !important;
  border-color: OrangeRed !important;
}

.btn {
  margin: 0 5px;
}
</style>
