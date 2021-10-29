<template>
  <div>
      <textarea :class="{'text-red': isWrong}" :disabled="disabled" @keyup="checkLettersAndReset(); handleBackspace($event)" v-model="textareaValue" cols="30" rows="10"></textarea>
      <p v-show="resetCount >= 3 ? true : false">Nothin for now</p>
      <button @click="restart">Renew</button>
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
            disabled: false,
            // result: {
            //     time: 0,
            //     mistakesCount: 0,
            //     mistakesLetter: ""
            // }
        }
    },
    methods: {
        checkLettersAndReset: function () {
            console.log(this.letterCombinations[this.pointer]);
            console.log(this.textareaValue[this.pointer]);
            if (this.textareaValue[this.pointer] !== this.letterCombinations[this.pointer]) {
                this.isWrong = true;
                this.textareaValue = this.textareaValue.substring(0, this.pointer);
            } else {
                this.pointer += 1;
                this.isWrong = false;
            }
            console.log(this.pointer);   
            
            if (this.pointer === this.letterCombinations.length) {
                this.pointer = 0;
                this.textareaValue = "";
                this.renew();
            }
        },
        handleBackspace: function (e) {
            if (e.key === 'Backspace') {
                this.pointer -= 1;
            }
        },
        renew: function () {
            this.resetCount += 1;
            if (this.resetCount >= 3) {
                this.disabled = true;
                console.log("no reset");
                return
            }
            this.$emit('renew');
        },
        restart: function () {
            this.resetCount = 0;
            this.disabled = false;
            this.renew();
        }
    }
}
</script>

<style>
    .text-red {
        color: red;
    }
</style>