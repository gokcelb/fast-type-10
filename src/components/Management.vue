<template>
  <div class="main">
      <Explanation :explanation='explanation'/>
      <WordContainer :letterCombinations='letterCombinations'/>
  </div>
</template>

<script>
import Explanation from './Explanation.vue'
import WordContainer from './WordContainer.vue'

export default {
    components: {
        Explanation,
        WordContainer,
    },
    data: function () {
        return {
            letterCombinations: '',
            combinations: [],
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
            for (let i=0; i<4; i++) {
                this.combinations.push(this.generateLetterCombination());
            }
            this.letterCombinations = this.combinations.join(" ");
        }
    },
    created () {
        this.combine();
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