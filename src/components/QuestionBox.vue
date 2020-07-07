<template>
  <div class="questino-box-container">
    <b-jumbotron>

      <template v-slot:lead>
        <p v-html="currentQuestion.question"> </p>
      </template>
      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer ,index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" 
        :class="answerClass(index)" >
        {{index}}: {{answer}}
        </b-list-group-item>
      </b-list-group>
    
  <br>
      <b-button variant="primary" @click="submitAnswer()" :disabled="selectedIndex === null || isSubmitted" > Submit</b-button>
      <b-button variant="success" @click="next"  :disabled="selectedIndex === null || !isSubmitted">  Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  data() {
     return {
       selectedIndex: null,
       shuffledAnswers: [],
       isSubmitted: false,
       isCorrect: null
     }
  },
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  methods:{
    answerClass(index){
      if(!this.isSubmitted && this.selectedIndex === index) 
        return 'selected-answer'
      else if(this.isSubmitted && this.selectedIndex === index && this.isCorrect)
        return 'correct'
      else if(this.isSubmitted && this.selectedIndex === index && !this.isCorrect)
        return 'incorrect'
      
      return ''
    },
    submitAnswer() {
      if (this.currentQuestion.correct_answer === this.shuffledAnswers[this.selectedIndex]){
        this.isCorrect = true
      }
      this.increment(this.isCorrect)
      this.isSubmitted = true
    },
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]  
      this.shuffledAnswers = _.shuffle(answers)
    }
  },
  watch: {
    currentQuestion:{
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers()
        this.isSubmitted = false
        this.isCorrect = null
      }
    }
  },
  computed: {
    answers(){
      let answers = this.currentQuestion.incorrect_answers 
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  }
}
</script>

<style scoped>
.list-group{
  margin: 5px;
}
.list-group-item:hover{
  background-color: #eeeeee;
  cursor: pointer;
}
.btn{
  margin: 0 5px;
}
.selected-answer{
  background-color: skyblue;
}
.correct{
  background-color: green;
}
.incorrect{
  background-color: red;
}
</style>