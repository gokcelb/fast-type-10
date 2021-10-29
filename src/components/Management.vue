<template>
  <div>
      <Explanation/>
      <Word v-bind:letterCombinations='letterCombinations'/>
      <Type v-bind:letterCombinations='letterCombinations' v-on:renew='createLetterCombinations()'/>
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
            letterCombinations: ""
        }
    },
    methods: {
        generateLetterCombination: function () {
            let letterCombination = "";
            const letters = "asdf";
            for (let i=0; i<4; i++) {
                letterCombination += letters[Math.floor(Math.random() * letters.length)]
            }
            return letterCombination;
        },
        createLetterCombinations: function () {
            const combinationsList = [];
            for (let i=0; i<4; i++) {
                combinationsList.push(this.generateLetterCombination());
            }
            this.letterCombinations = combinationsList.join(" ");
        }
    },
    mounted () {
        this.createLetterCombinations();
    }
}
</script>

<style>

</style>