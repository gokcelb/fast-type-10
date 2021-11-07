<template>
  <div>
    <p class="text-24">
      <span class="text-green">{{ convertToString(doneWords) }}</span>
      <span class="text-green">{{ convertToString(doneLetters) }}</span>
      <span
        class="text-bg-red"
        v-for="(mistake, index) in mistakes"
        :key="index"
        >{{ mistake["expectedLetter"] }}</span
      >
      <span>{{ remaining }}</span>
    </p>
    <typing-container
      :preventTyping="preventTyping"
      ref="typingContainer"
      @onType="onType"
      @onBackspace="onBackspace"
      @initAnalysis="initAnalysis"
      @endAnalysis="endAnalysis"
    ></typing-container>
    <analysis-container
      ref="analysisContainer"
      :roundOver="roundOver"
    ></analysis-container>
    <div>
      <button @click="reset" v-if="roundOver">Reset</button>
    </div>
  </div>
</template>

<script>
import TypingContainer from "./TypingContainer.vue";
import AnalysisContainer from "./AnalysisContainer.vue";

export default {
  components: {
    TypingContainer,
    AnalysisContainer,
  },
  props: {
    letterCombinations: {
      type: String,
    },
    combinations: {
      type: Array,
    },
  },
  data: function () {
    return {
      doneLetters: [],
      doneWords: [],
      mistakes: [],
      remaining: this.letterCombinations,
      roundOver: false,
      preventTyping: false,
      typeCount: 0,
      analysisInitialized: false,
      wordTimerInitialized: false,
    };
  },
  methods: {
    onType: function (event, typedLetter) {
      this.typingStarting();
      this.typingContinuing(typedLetter);
      this.typingEnding(event);
      console.log({
        "done letters": this.doneLetters,
        mistakes: this.mistakes,
        "done words": this.doneWords,
        "remaining letters": this.remaining,
      });
    },
    typingStarting: function () {
      this.typeCount++;
      if (!this.wordTimerInitialized) {
        this.$refs.analysisContainer.timeWordTypingSpeed(this.doneWords.length);
        this.wordTimerInitialized = true;
      }
      if (this.remaining === "") {
        this.preventTyping = true;
      }
    },
    typingContinuing: function (typedLetter) {
      // end function when remaining has undefined element
      if (this.remaining[0] === undefined) return;
      let expectedLetter = this.remaining[0];
      if (this.mistakes.length > 0 || expectedLetter !== typedLetter) {
        this.mistakes.push({
          expectedLetter: expectedLetter,
          mistypedLetter: typedLetter,
        });
        this.$refs.analysisContainer.trackMistakes(expectedLetter, typedLetter);
      } else if (expectedLetter === typedLetter) {
        this.doneLetters.push(expectedLetter);
      }
      this.$refs.analysisContainer.calculateAccuracy(this.typeCount);
      if (this.mistakes.length === 1 && this.doneLetters.length > 0) {
        this.$refs.analysisContainer.analyzeConsecutiveLetters(
          this.mistakes[0]["expectedLetter"],
          this.doneLetters[this.doneLetters.length - 1]
        );
      }
      this.remaining = this.remaining.substring(1);
    },
    typingEnding: function (event) {
      if (this.remaining === "" && this.mistakes.length === 0) {
        this.roundOver = true;
        this.$refs.typingContainer.disableTextarea();
      }
      if (
        (event.code === "Space" &&
          this.mistakes.length === 0 &&
          this.doneLetters.includes(" ")) ||
        this.roundOver
      ) {
        const doneWord = this.doneLetters.join("");
        this.doneWords.push(doneWord);
        this.$refs.analysisContainer.stopTimingWordTypingSpeed(
          doneWord,
          this.doneWords.length - 1
        );
        this.wordTimerInitialized = false;
        this.doneLetters = [];
        this.$refs.typingContainer.clearTypedLetters();
      }
    },
    onBackspace: function (index) {
      console.log(`went into on backspaceeeee`);
      this.preventTyping = false;
      if (this.mistakes.length > 0) {
        console.log(`went into if`);
        const lastMistake = this.mistakes.pop();
        const lastMistakenLetter = lastMistake["expectedLetter"];
        this.remaining = lastMistakenLetter + this.remaining;
        console.log("after deletion :", this.mistakes, index);
      } else if (this.doneLetters.length > 0) {
        const lastDoneLetter = this.doneLetters.pop();
        this.remaining = lastDoneLetter + this.remaining;
        console.log("after deletion :", this.doneLetters, index);
      }
    },
    initAnalysis: function () {
      if (this.analysisInitialized) {
        return;
      }
      this.$refs.analysisContainer.analyze();
      this.analysisInitialized = true;
    },
    endAnalysis: function () {
      this.$refs.analysisContainer.stopAnalyzing();
    },
    // reset: function () {
    //   this.$emit("reset");
    // },
    convertToString: function (list) {
      return list.join("");
    },
  },
};
</script> 

<style>
.text-green {
  color: rgb(58, 206, 58);
}
.text-bg-red {
  background-color: red;
}
.text-24 {
  font-size: 24px;
}
.bg-grey {
  background-color: gainsboro;
}
</style>