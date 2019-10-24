<template>
  <div class="heading">
    <h1 class="heading__main">Quiz App</h1>
    <p class="heading__text">
      A simple quiz app built with <code>Vue CLI</code>.
    </p>
    <h6 class="heading__sub">
      Score: {{ numCorrectAnswers }} / {{ numQuestions }}
    </h6>
  </div>
</template>

<script>
export default {
  props: {
    numQuestions: {
      type: Number,
      default: 0
    },

    numCorrectAnswers: {
      type: Number,
      default: 0
    }
  }
};
</script>

<style lang="scss" scoped>
.heading {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;

  &__main {
    color: #fff;
  }

  &__sub {
    color: #fff;
  }

  &__text {
    color: #fff;
  }
}
</style>
