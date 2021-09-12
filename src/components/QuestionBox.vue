<template>
    <div class="question-box-container"> 
        <b-jumbotron>
            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer,index) in shuffledAnswers" 
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="itemClass(index)">
                     {{answer}}
               </b-list-group-item>
            </b-list-group>
       

            <b-button 
                variant="primary"
                @click="sumbitAnswer()"
                :disabled="this.selectedIndex==null || answered===true"
                >Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
    
</template>

<script>
import _ from 'lodash'

export default {
   props:{
       currentQuestion: Object,
       next:Function,
       increment:Function

   },
   data(){
       return {
           selectedIndex: null,
           correctIndex: null,
           shuffledAnswers: null,
           answered:false
       }
   },
//    computed: {
//        answers: function(){
//            let answers = [...this.currentQuestion.incorrect_answers]
//            answers.push(this.currentQuestion.correct_answer)
//            return answers
//        }
//    },
   watch: {
       currentQuestion: {
           immediate: true,
           handler(){
               this.selectedIndex = null
               this.shuffleAnswers()
               this.answered=false
           }
       }
   },
   methods: {
       selectAnswer: function(index){
           this.selectedIndex = index
       },
       shuffleAnswers: function(){
           let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
           this.shuffledAnswers = _.shuffle(answers)
           this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
       },
       sumbitAnswer: function(){
           console.log(this.selectedIndex)
           console.log(this.correctIndex)
           this.increment(this.selectedIndex===this.correctIndex)
           this.answered = true

       },
       itemClass: function(index){
           if(this.selectedIndex===index&&this.answered==false){
               return 'selected'
           }else if(this.answered&&this.correctIndex===index){
               return 'correct'
           }else if(this.answered&&this.selectedIndex===index && this.correctIndex!==index){
               return 'incorrect'
           }
       }
   }
}
</script>

<style scoped>
.list-group {
    margin-bottom: 15px;
}
.list-group-item:hover {
    background: #EEE;
    cursor: pointer;
}
.btn {
    margin: 0 5px;
}

.selected {
    background-color: lightblue;
}

.correct {
    background-color: green;
}

.incorrect {
    background-color: red;
}


</style>