<template>
  <div id="app">
    <Header
        v-bind:numCorrect="numCorrect"
        v-bind:numTotal="numTotal"
    />

    <b-container class="bv-example-row col-sm-5">
      <b-row>
        <b-col>
          <QuestionBox
                  v-if="questions.length"
                  v-bind:currentQuestion="questions[index]"
                  v-bind:next="next"
                  v-bind:increment="increment"
                  v-bind:currentQuestionIndex="index"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
        if (isCorrect) {
            this.numCorrect++
        }
        this.numTotal++
    }
  },
  mounted: function () {
    fetch('https://opentdb.com/api.php?amount=10&category=23&type=multiple', {
      method: 'get'
    })
            .then((response) => {
              let jsonResponse = response.json()
              // eslint-disable-next-line no-console
              console.log(jsonResponse)
              return jsonResponse
            })
            .then((jsonData) => {
              this.questions = jsonData.results
            })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
