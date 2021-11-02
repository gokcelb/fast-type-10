<template>
  <div>
    <input
      @keyup="onType"
      v-model="typedLetters"
      type="text"
      :disabled="disabled"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      typedLetters: "",
      index: 0,
      disabled: false,
    };
  },
  methods: {
    onType: function (event) {
      if (event.key === "Backspace") {
        this.onBackspace();
      } else {
        let typedLetter = this.typedLetters[this.index];
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
      this.typedLetters = '';
      this.disabled = true;
    },
  },
};
</script>

<style>
</style>