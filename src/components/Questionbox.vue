<template>

  <div class="question-box-container">
    <b-jumbotron>


      <template v-slot:lead>
  {{ currentQuestion.question}}
      </template>

      <hr class="my-4">

<!-- //this takes the incorrect answers api array  -->
<!-- //:class="[selectedIndex === index ? 'selected' :'']"> is used to if else your css class, useful if you want to change a particular style of an element dynamically  -->

<b-list-group >
  <b-list-group-item
  v-for="(answer,index) in shuffledAnswers" :key="index"

 @click="selectanswer(index)"
 :class="answerClass(index)"
>
{{answer}}

 </b-list-group-item>

</b-list-group>


<div class="mt-3">
      <b-button class="mr-3 " variant="primary"
      @click="submitAnswer"
      :disabled="selectedIndex === null || answered"
      >Submit

    </b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </div>

    </b-jumbotron>
  </div>
</template>


<script>

//required this for every proerpty in the javascript

import _ from 'lodash'

export default{

  data(){
    return {
      selectedIndex:null,
       correctIndex: null,
          shuffledAnswers: [],
          answered: false
    }
  },

  props: { //is required to connect your main component with other components
    currentQuestion: Object,
    next: Function,
    increment: Function
  },

  computed: {
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers] // take the values of array from appi and put in the answers array
        answers.push(this.currentQuestion.correct_answer) //push another value
        return answers //return it
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
    },

methods:{ //just like your javasscript fucntions

  selectanswer(index){
    this.selectedIndex=index
  },


  submitAnswer() {
     let isCorrect = false
     if (this.selectedIndex === this.correctIndex) {
       isCorrect = true
     }
     this.answered = true
     this.increment(isCorrect) //a method to send to the app vue so data can travel from the component to the app
   },

shuffleAnswers(){
  let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
this.shuffledAnswers = _.shuffle(answers)
this.correctIndex= this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)

},

answerClass(index){

      let answerClass=''

      if(  !this.answered && this.selectedIndex === index ){   //need to refercen javascript by using this unless it is not parsed through the method
        answerClass= 'selected'
      } else if (this.answered && this.correctIndex === index ){
        answerClass= 'correct'
      }
       else if ( this.answered && this.selectedIndex===index && this.correctIndex !== index){
         answerClass='incorrect'
       }

  return answerClass

}


}
}


</script>


<style lang="scss" scoped>
.list-group{
  color:color(black);

}

.list-group-item:hover{
  // background:color(green);
  cursor: pointer;
}

.selected{
    background-color:color(green);
}

.incorrect{
  background:color(danger);
}

.correct{
  background:color(primary);
}

</style>
