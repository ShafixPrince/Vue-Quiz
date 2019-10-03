<template>
  <div id="app">
<Header

:numCorrect="numCorrect"
:numTotal="numTotal"
 />

<b-container class="bv-example-row text-center">
  <b-row>

    <!-- this will bind your question box component, this uses v-bind and questions from the array in the api  -->
    <b-col sm="6" lg="12"> <Questionbox
 v-if="questions.length"
    :currentQuestion="questions[index]"
    :next="next"
    :increment="increment"

    />

  </b-col>

  </b-row>
</b-container>



  </div>
</template>

<script>


// {{}} is used to put your data inside from the javascript
// components should be put in the components {}
// methods: are used to put your methods inside the same as on click on javasscirpt and jquery
// vue lifecycle is used mount, destroy, create, etc
import Header from './components/Header.vue'
import Questionbox from './components/Questionbox.vue'



export default {


  name: 'app',
  components: {
    Header,
    Questionbox
  },

  data(){
    //this return is connectd to the next method below
    return {
      questions: [],
      index: 0,
      numCorrect:0,
      numTotal: 0
    }

  },


  methods: {
    next(){
      this.index++
    },

    increment(isCorrect) { //this is from Question box component, from here you can props it to the header vue so that the value of header vue changes. 
         if (isCorrect) {
           this.numCorrect++
         }
         this.numTotal++
       }
  },

  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple', {
      method: 'get'
    })

    .then((response) => {
     return response.json()
    })

    .then((jsonData) => {
     this.questions = jsonData.results
    })
  }

}



</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: color(green);
  margin-top: 60px;
}
</style>
