<template>
  <div>
    <textarea :class="{'text-red': isWrong}" :disabled="disabled" @click="isOn ? null : calculateDuration()" @keyup="checkLettersAndReset(); handleBackspace($event)" v-model="textareaValue" cols="30" rows="10"></textarea>
    <div v-show="resetCount > 3 ? true : false">
        <p>Duration: {{ analysis.duration }} seconds</p>
        <p>Mistake Count: {{ analysis.mistakenLetters.length }}</p>
        <p v-for="(value, name) in rates" v-bind:key="name">Mistaken Letters: {{ name }} -> {{ value }}%</p>
    </div>
    <div>
        <button v-show="resetCount > 3 ? true : false" @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
    props: {
        letterCombinations: {
            type: String
        }
    },
    data: function () {
        return {
            textareaValue: "",
            pointer: 0,
            isWrong: false,
            resetCount: 0,
            t: 0,
            isOn: false,
            disabled: false,
            analysis: {
                duration: 0,
                mistakenLetters: []
            },
            rates: {}
        }
    },
    methods: {
        // check if letters match with every keyup, process if letter wrong, start reset process
        checkLettersAndReset: function () {
            let currLetter = this.letterCombinations[this.pointer]
            if (this.textareaValue[this.pointer] !== currLetter) {
                this.isWrong = true;
                this.textareaValue = this.textareaValue.substring(0, this.pointer);
                this.analysis.mistakenLetters.push(currLetter);
            } else {
                this.pointer += 1;
                this.isWrong = false;
            }
            
            // check if app should trigger generate function and initialize reset process
            if (this.pointer === this.letterCombinations.length) {
                this.pointer = 0;
                this.textareaValue = "";
                this.generate();
            }
        },
        // calculate duration starting from click on textarea until end of combinations
        calculateDuration: function () {
            this.isOn = true; // to prevent double clicking and hence, function build-up
            this.t = setInterval(() => {
                if (this.resetCount > 3) {
                    clearInterval(this.t);
                    this.isOn = false;
                }
                this.analysis.duration += 1;
            }, 1000);
        },
        // decrease pointer by one with backspace keyup
        handleBackspace: function (e) {
            if (e.key === 'Backspace') {
                this.pointer -= 1;
            }
        },
        // emit generate function to trigger combine function in Management
        generate: function () {
            this.resetCount += 1;
            if (this.resetCount > 3) {
                this.disabled = true; // disabling text are to prevent user input
                this.calculatedRates;
                return
            }
            this.$emit('generate');
        },
        // reset values and call generate
        reset: function () {
            this.analysis.mistakenLetters = [];
            this.rates = {}
            this.analysis.duration = 0;
            this.resetCount = 0;
            this.disabled = false;
            this.generate();
        },
        // turn mistakenLetters array to dictionary, calculate mistaken letter percentages
        calculateMistakenLetterRates: function (a) {
            for (let i = 0; i < a.length; i++) {
                if (a[i] === " ") {
                    a[i] = "space";
                }
                if (this.rates[a[i]]) {
                    this.rates[a[i]] += 1;
                } else {
                    this.rates[a[i]] = 1;
                }
            }
            for (let key in this.rates) {
                this.rates[key] = Math.round((this.rates[key] / a.length) * 100);
            }
        }
    },
    computed: {
        calculatedRates () {
            return this.calculateMistakenLetterRates(this.analysis.mistakenLetters);
        }
    },
    mounted () {
        this.generate();
    }
}
</script>

<style>
    .text-red {
        color: red;
    }
</style>