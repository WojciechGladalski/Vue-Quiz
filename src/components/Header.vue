<template>

    <div>
        <b-nav tabs align="center">
            <b-nav-item disabled>Vue History Quiz</b-nav-item>
            <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item>
        </b-nav>
    </div>

</template>

<script>
    export default {
        name: "Header",
        props: [
            'numCorrect',
            'numTotal'
        ]
    }
</script>

<style scoped>
    .nav-item {
        font-size: 2em;
    }
</style>