<template>

    <div>
        <b-jumbotron>
            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                        v-for="(answer, index) in shuffledAnswers"
                        v-bind:key="index"
                        @click="answered === false && submitAnswer(index)"
                        v-bind:class="answerClass(index)"
                >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button
                    variant="success"
                    @click="next"
                    v-bind:disabled="answered === false || currentQuestionIndex >= 9"
            >
                Next question
            </b-button>
        </b-jumbotron>
    </div>

</template>

<script>
    import _ from 'lodash'

    export default {
        name: "QuestionBox",
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function,
            currentQuestionIndex: Number
        },
        methods: {
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer(index) {
                this.selectedIndex = index

                let isCorrect = false

                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true

                this.increment(isCorrect)
            },
            answerClass(index) {

                let answerClass = 'waiting'

                if (this.answered && this.correctIndex === index) {
                    answerClass = 'correct'
                } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                    answerClass = 'incorrect'
                } else if (this.answered) {
                    answerClass = 'mute'
                }
                return answerClass
            }
        },
        data: function() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleAnswers()
                }
            }
        }
    }
</script>

<style scoped>

    .list-group {
       margin-bottom: 16px;
    }
    .waiting:hover {
        background: #EEE;
        cursor: pointer;
    }
    .btn {
        margin: 0 6px;
    }
    .correct {
        background-color: greenyellow;
    }
    .incorrect {
        background-color: red;
    }

</style>