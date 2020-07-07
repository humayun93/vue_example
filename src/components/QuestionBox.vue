<template>
  <div class="questino-box-container">
    <b-jumbotron>

      <template v-slot:lead>
        <p v-html="currentQuestion.question"> </p>
      </template>
      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer ,index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" 
        :class="[selectedIndex === index ? 'selected-answer' : '']" >
        {{index}}: {{answer}}
        </b-list-group-item>
      </b-list-group>
    
  <br>
      <b-button variant="primary" href="#"> Submit</b-button>
      <b-button variant="success" href="#" @click="next" > Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  data() {
     return {
       selectedIndex: null,
       shuffledAnswers: []
     }
  },
  props: {
    currentQuestion: Object,
    next: Function
  },
  methods:{
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