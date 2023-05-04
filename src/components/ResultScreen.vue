<template>
    <div class="screen">
        <h1>Congratulations</h1>
        <p>{{ Math.round(timer / 1000) }} seconds</p>
        <button @click="startAgain()">Start Again</button>
    </div>
</template>

<script>
export default {
    props: {
        timer: {
            type: Number,
            required: true
        }
    },
    methods: {
        startAgain() { // Bắt đầu màn chơi mới
            this.$emit('startAgain')
        }

    },
}
</script>