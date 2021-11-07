<template>
  <div>
    <p>Duration: {{ elapsedTime }}</p>
    <p>Total mistake count: {{ mistakes.length }}</p>
    <p>Accuracy: {{ accuracy }}%</p>
    <p v-for="(mistake, index) in mistakes" :key="'m-' + index">
      Typed
      <span
        ><strong>{{ mistake["mistypedLetter"] }}</strong></span
      >
      instead of
      <span
        ><strong>{{ mistake["expectedLetter"] }}</strong></span
      >
    </p>
    <p
      v-for="(consecutiveLetterMistake, index) in consecutiveLetterMistakes"
      :key="'clm-' + index"
    >
      When
      <span
        ><strong>{{ consecutiveLetterMistake["expectedLetter"] }}</strong></span
      >
      followed
      <span
        ><strong>{{ consecutiveLetterMistake["previousLetter"] }}</strong></span
      >, you made a mistake
    </p>
    <p v-for="(values, index) in wordTimer" :key="'wt-' + index">
      <span><strong>{{ values["word"] }}</strong></span>: {{ values["wordTypingSpeed"] }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    roundOver: {
      type: Boolean
    }
  },
  data() {
    return {
      mistakes: [],
      accuracy: 0,
      startTime: 0,
      elapsedTime: 0,
      consecutiveLetterMistakes: [],
      wordTimer: [],
    };
  },
  methods: {
    trackMistakes: function (expectedLetter, mistypedLetter) {
      expectedLetter = expectedLetter === " " ? "space" : expectedLetter;
      mistypedLetter = mistypedLetter === " " ? "space" : mistypedLetter;
      this.mistakes.push({
        expectedLetter: expectedLetter,
        mistypedLetter: mistypedLetter,
      });
    },
    calculateAccuracy: function (typeCount) {
      this.accuracy = 100 - (this.mistakes.length * 100) / typeCount;
    },
    analyze: function () {
      console.log("ANALYSIS STARTEED BABY");
      this.startTime = new Date();
    },
    stopAnalyzing: function () {
      console.log("ANALYSIS OVER YOO");
      const endTime = new Date();
      this.elapsedTime = (endTime.getTime() - this.startTime.getTime()) / 1000;
    },
    timeWordTypingSpeed: function () {
      console.log("TIMING WORD");
      const timerStart = new Date();
      this.wordTimer.push({
        timerStart: timerStart,
      });
    },
    stopTimingWordTypingSpeed: function (word, index) {
      console.log("TIMING STOOPPPED");
      const timerEnd = new Date();
      this.wordTimer[index]["timerEnd"] = timerEnd;
      const wordTypingSpeed =
        (this.wordTimer[index]["timerEnd"].getTime() -
          this.wordTimer[index]["timerStart"].getTime()) /
        1000;
      this.wordTimer[index]["wordTypingSpeed"] = wordTypingSpeed;
      this.wordTimer[index]["word"] = word;
      console.log(this.wordTimer);
    },
    analyzeConsecutiveLetters: function (expectedLetter, previousLetter) {
      expectedLetter = expectedLetter === " " ? "space" : expectedLetter;
      previousLetter = previousLetter === " " ? "space" : previousLetter;
      this.consecutiveLetterMistakes.push({
        expectedLetter: expectedLetter,
        previousLetter: previousLetter,
      });
    },
  },
};
</script>

<style>
</style>