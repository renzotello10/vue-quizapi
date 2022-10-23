<template>
  <Header  
  :numCorrect="numCorrect"
  :numTotal="numTotal" />
  <!--Quizbox v-if="questions.length"
                :currentQuestion="questions[index]"
                :next="next"/-->
<Quizbox v-if="questions.length"
         :currentQuestion="questions[index]"
         :next="next"
         :increment="increment" />                
</template>

<script>

import Header from './components/Header.vue'
import Quizbox from './components/Quizbox.vue'

export default {
  name: 'App',
  components: {
    Header,
    Quizbox
  },
  data() {
    return {
      questions:[],
      index : 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods:{
     getQuestion(){
       fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple",{
        method:'GET'
       }).then(response => {
          return response.json()
       }).then(jsonData =>{
         this.questions = jsonData.results 
       }).catch(error =>{
         console.error('Panic Error:',error)
       })
    
      },
      next() {
      this.index++
      },
      increment (isCorrect) {
        if (isCorrect) {
          this.numCorrect++
        }
        this.numTotal++
      }
     
  },
  mounted: function() {
    this.getQuestion();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
