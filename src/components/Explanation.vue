<template>
    <div>
        <img class = "keyboard-img" src="../assets/colored-keyboard.png" alt="Colored Keyboard">
         <p class="explanation">{{ explanation }}</p>
    </div>
</template>

<script>
export default {
    props: {
        explanation: {
            type: String
        }
    }
}
</script>

<style>
    .keyboard-img {
        display: flex;
    }
    .explanation {
        display: float;
    }

</style>