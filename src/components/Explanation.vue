<template>
    <div>
        <img src="../assets/colored-keyboard.png" alt="">
         <p>{{ explanation }}</p>
    </div>
</template>

<script>
export default {
    data: function () {
        return {
            explanation: `Your left fingers should continuously be placed on the ASDF row.
            Your index finger should be on F, middle finger on D, ring finger on S, and
            pinky finger on A.`
        }
    }
}
</script>

<style>

</style>