<template>
  <div>
    <input
      @keydown="onType"
      v-model="typedLetters"
      type="text"
      :disabled="disabled"
      :readonly="preventTyping"
    />
  </div>
</template>

<script>
export default {
  props: {
    // letterCombinations: {
    //   type: String
    // },
    // remaining: {
    //   type: String
    // },
    preventTyping: {
      type: Boolean
    }
  },
  data() {
    return {
      typedLetters: "",
      index: 0,
      disabled: false,
    };
  },
  methods: {
    onType: function (event) {
      // if backspace is clicked call onBackspace function,
      // else check if preventTyping should be activated,
      // if not, proceed with standard typing procedures
      if (event.key === "Backspace") {
        this.onBackspace();
      } else {
        if (this.preventTyping) return;
        let typedLetter = event.key;
        console.log(typedLetter, this.index);
        this.$emit("onType", event, typedLetter);
        this.index++;
      }
    },
    onBackspace: function () {
      // if index is equal to 0, meaning no letter has been
      // typed on the textarea, end the function, otherwise,
      // emit onBackspace function to be called in the WordContainer
      console.log("backspaaaaaaaaaace");
      if (this.index === 0) return;
      this.$emit("onBackspace", this.index);
      this.index--;
    },
    clearTypedLetters: function () {
      // everytime user types one combination correctly,
      // clear textarea and reset index
      this.typedLetters = "";
      this.index = -1;
    },
    disableTextarea: function () {
      // when round is over, disable the textarea
      this.typedLetters = "";
      this.disabled = true;
    },
  },
};
</script>

<style>
</style>