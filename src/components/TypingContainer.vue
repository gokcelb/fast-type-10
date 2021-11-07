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
    preventTyping: {
      type: Boolean
    },
  },
  data() {
    return {
      typedLetters: "",
      index: 0,
      disabled: false,
      analysisOn: false
    };
  },
  methods: {
    onType: function (event) {
      this.analysisOn = true;
      this.$emit('initAnalysis');
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
      console.log("backspaaaaaaaaaace");
      if (this.index === 0) return;
      this.$emit("onBackspace", this.index);
      this.index--;
    },
    clearTypedLetters: function () {
      this.typedLetters = "";
      this.index = -1;
    },
    disableTextarea: function () {
      this.analysisOn = false;
      this.$emit('endAnalysis');
      this.typedLetters = "";
      this.disabled = true;
    },
  },
};
</script>

<style>
</style>