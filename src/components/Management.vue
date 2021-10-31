<template>
  <div class="main">
      <Explanation :explanation='explanation'/>
      <Word :letterCombinations='letterCombinations'/>
      <Type :letterCombinations='letterCombinations' v-on:generate='combine()'/>
  </div>
</template>

<script>
import Explanation from './Explanation.vue'
import Word from './Word.vue'
import Type from './Type.vue'

export default {
    components: {
        Explanation,
        Word,
        Type
    },
    data: function () {
        return {
            letterCombinations: "",
            explanation: `Your left fingers should continuously be placed on the ASDF row.
            Your index finger should be on F, middle finger on D, ring finger on S, and
            pinky finger on A.`
        }
    },
    methods: {
        // create one letter combination consisting of 4 random letters
        generateLetterCombination: function () {
            let letterCombination = "";
            const letters = "asdf";
            for (let i=0; i<4; i++) {
                letterCombination += letters[Math.floor(Math.random() * letters.length)]
            }
            return letterCombination;
        },
        // combine letter combinations and add spaces between them
        combine: function () {
            const combinations = [];
            for (let i=0; i<4; i++) {
                combinations.push(this.generateLetterCombination());
            }
            this.letterCombinations = combinations.join(" ");
        }
    }
}
</script>

<style>
    .main {
        height: 75%;
        margin-top: 2%;
        margin-bottom: 2%;
        padding: 1%;
    }
</style>