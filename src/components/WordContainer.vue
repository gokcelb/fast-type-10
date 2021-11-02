<template>
  <div class="text-24">
    <p>
      <span class="text-green">{{ convertToString(doneWords) }}</span>
      <span class="text-green">{{ convertToString(doneLetters) }}</span>
      <span class="text-bg-red">{{ convertToString(mistakenLetters) }}</span>
      <span>{{ remaining }}</span>
    </p>
    <TypingContainer
      ref="typingContainer"
      @onType="onType"
      @onBackspace="onBackspace"
    />
  </div>
</template>

<script>
import TypingContainer from "./TypingContainer.vue";

export default {
  components: {
    TypingContainer,
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
      mistakenLetters: [],
      mistypedLetters: [],
      remaining: this.letterCombinations,
    };
  },
  methods: {
    onType: function (event, typedLetter) {
      const expectedLetter = this.remaining[0];
      if (this.mistakenLetters.length > 0 || expectedLetter !== typedLetter) {
        this.mistakenLetters.push(expectedLetter);
        this.mistypedLetters.push(typedLetter);
      } else if (expectedLetter === typedLetter) {
        this.doneLetters.push(expectedLetter);
      }
      this.remaining = this.remaining.substring(1);
      if (
        event.code === "Space" &&
        this.mistakenLetters.length === 0 &&
        this.doneLetters.includes(" ")
      ) {
        this.doneWords.push(...this.doneLetters);
        this.doneLetters = [];
        this.$refs.typingContainer.clearTypedLetters();
      }
      if (this.remaining === '' && this.mistakenLetters.length === 0)  {
        this.$refs.typingContainer.disableTextarea();
      }
      console.log({
        "done letters": this.doneLetters,
        "mistaken letters": this.mistakenLetters,
        "mistyped letters": this.mistypedLetters,
        "done words": this.doneWords,
        "remaining letters": this.remaining,
      });
    },
    onBackspace: function (index) {
      console.log(`went into on backspaceeeee`);
      if (this.mistakenLetters.length > 0) {
        console.log(`went into if`);
        const lastMistakenLetter = this.mistakenLetters.pop();
        this.remaining = lastMistakenLetter + this.remaining;
        console.log("after deletion :", this.mistakenLetters, index);
      } else {
        const lastDoneLetter = this.doneLetters.pop();
        this.remaining = lastDoneLetter + this.remaining;
        console.log("after deletion :", this.doneLetters, index);
      }
    },
    convertToString: function (list) {
      return list.join("");
    },
  },
};
</script> 

<style>
.text-green {
  color: green;
}
.text-bg-red {
  background-color: red;
}
.text-24 {
  font-size: 24px;
}
</style>