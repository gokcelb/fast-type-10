<template>
  <div class="text-24">
    <p>
      <span class="text-green">{{ convertToString(doneWords) }}</span>
      <span class="text-green">{{ convertToString(doneLetters) }}</span>
      <span class="text-bg-red">{{ convertToString(mistakenLetters) }}</span>
      <span>{{ remaining }}</span>
    </p>
    <TypingContainer
      :preventTyping="preventTyping"
      ref="typingContainer"
      @onType="onType"
      @onBackspace="onBackspace"
    />
    <div>
      <button @click="reset" v-if="roundOver">Reset</button>
    </div>
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
      roundOver: false,
      preventTyping: false,
    };
  },
  methods: {
    onType: function (event, typedLetter) {
      // activate preventTyping if remaining is empty
      if (this.remaining === "") {
        this.preventTyping = true;
      }
      // end function when remaining has undefined element
      // to prevent it from appearing on the screen
      if (this.remaining[0] === undefined) return;

      let expectedLetter = this.remaining[0];
      if (this.mistakenLetters.length > 0 || expectedLetter !== typedLetter) {
        this.mistakenLetters.push(expectedLetter);
        this.mistypedLetters.push(typedLetter);
      } else if (expectedLetter === typedLetter) {
        this.doneLetters.push(expectedLetter);
      }

      // once expectedLetter or typedLetter are pushed,
      // delete first element of remaining
      this.remaining = this.remaining.substring(1);

      // trigger pushing typed word to doneWords, empty doneLetters
      // and call the clearTypedLetters function in TypingContainer
      if (
        event.code === "Space" &&
        this.mistakenLetters.length === 0 &&
        this.doneLetters.includes(" ")
      ) {
        this.doneWords.push(...this.doneLetters);
        this.doneLetters = [];
        this.$refs.typingContainer.clearTypedLetters();
      }

      // end round and trigger disableTextarea function in child component
      if (this.remaining === "" && this.mistakenLetters.length === 0) {
        this.roundOver = true;
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
      // once backspace is clicked disable preventTyping
      this.preventTyping = false;

      if (this.mistakenLetters.length > 0) {
        console.log(`went into if`);
        const lastMistakenLetter = this.mistakenLetters.pop();
        this.remaining = lastMistakenLetter + this.remaining;
        console.log("after deletion :", this.mistakenLetters, index);
      } else if (this.doneLetters.length > 0) {
        const lastDoneLetter = this.doneLetters.pop();
        this.remaining = lastDoneLetter + this.remaining;
        console.log("after deletion :", this.doneLetters, index);
      }
      console.log({
        "done letters": this.doneLetters,
        "mistaken letters": this.mistakenLetters,
        "mistyped letters": this.mistypedLetters,
        "done words": this.doneWords,
        "remaining letters": this.remaining,
      });
    },
    reset: function () {
      this.$emit("reset");
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