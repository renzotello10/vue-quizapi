<template>
    <div class="question-box-container">
        <h3>
            <b> {{currentQuestion.question}} </b>
        </h3>
        <div class="center">
        <ol class="center list-group">
              <li class="list-group-item"
              v-for="(answer, index) in shuffledAnswers" :key="index"
              @click.prevent="selectAnswer(index)"
              :class="answerClass(index)"
              >{{answer}}</li>
    
        </ol>
       </div>
        <div class="vstack gap-2 col-md-5 mx-auto">
            <button type="button" @click="submitAnswer"
            :disabled="selectedIndex === null || answered"
            class="btn btn-secondary">Submit</button>
            <button @click="next" type="button" class="btn btn-outline-secondary">Next</button>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props:{
        currentQuestion:Object,
        next: Function,
        increment:Function
    },
    data() {
        return {
            selectedIndex : null,
            correctIndex:null,
            shuffledAnswers: [],
            answered: false

        }
    },
    computed:{
        answers(){
            const answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }        
    },
    methods: {
        selectAnswer(index){
          this.selectedIndex = index
        },
        submitAnswer(){
            let isCorrect = false
            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        shuffleAnswers () {
            const answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass (index) {
            let answerClass = ''
            if (!this.answered && this.selectedIndex === index) {
            answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
            answerClass = 'correct'
            } else if (this.answered &&
                this.selectedIndex === index &&
                this.correctIndex !== index
            ) {
            answerClass = 'incorrect'
            }
        return answerClass
        } 
    },
    watch:{
        currentQuestion:{
            immediate:true,
            handler(){
               this.selectedIndex = null
               this.answered = false
               this.shuffleAnswers()
                    
            }
        }
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
    width: 50%;
    align-content: center;
 }
 .center {
    margin: auto;
    width: 60%;
    border: 0px solid #73AD21;
    padding: 10px;
 }
.list-group-item:hover {
 background: yellow;
 cursor:pointer;
}
.selected {
    background-color: yellowgreen;
}

.correct{
    background-color: green
 }
 .incorrect{
    background-color: red;
 }
</style>