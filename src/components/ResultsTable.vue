<template>
  <div class="result-container">
    <p>✅ Correct guesses: {{ correctGuesses }}</p>
    <p>❌ Wrong guesses: {{ wrongGuesses }}</p>
    <p>🎯 Accuracy: {{ accuracy }}%</p>
    <button type="button" class="reset-button" @click="$emit('reset')">
      Reset
    </button>
    <button type="button" class="retry-button" @click="$emit('retry')">
      Retry
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    correctGuesses: {
      type: Number,
    },
    wrongGuesses: {
      type: Number,
    },
  },
  computed: {
    accuracy() {
      return Math.round(
        100 * (this.correctGuesses / (this.wrongGuesses + this.correctGuesses))
      );
    },
  },
};
</script>

<style scoped></style>
