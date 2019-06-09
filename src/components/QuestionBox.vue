<template>
  <div class="question-box-container">
    <b-jumbotron>

    <template slot="lead">
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item v-for="(answer,index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" :class="answeredClass(index)">
        {{answer}}
      </b-list-group-item>
    </b-list-group>

    <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex===null|| answered">Submit</b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
     </b-jumbotron>
  </div>
</template>

<script>

import _ from 'lodash'//_ is the convention for loading lodash

export default {
  props:{
    currentQuestion:Object,
    next:Function,
    increment:Function
  },
  data:function(){
    return {
      selectedIndex:null,
      correctIndex:null,
      shuffledAnswers:[],
      answered:false
    };
  },
   computed:{
    answers(){
      let answers=[...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch:{
    //It is for shuffling the answers, instead of this mounted hook can also be used
    currentQuestion:{
      immediate:true,//it allows the changes to occur right from the staring of program
      handler(){
        this.selectedIndex=null;
        this.answered=false;
        this.shuffleAnswers();
      }
    }
    
    // (){
    //   this.selectedIndex=null;
    //   this.shuffleAnswers();
    // }
  },
  methods:{
    selectAnswer(index){
      this.selectedIndex=index;
      console.log(index);
    },
    submitAnswer(){
      let isCorrect=false;
      if(this.selectedIndex===this.correctIndex){
        isCorrect=true;
      }
      this.answered=true;
      this.increment(isCorrect);
    },
    shuffleAnswers(){
      let answers=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
      this.shuffledAnswers=_.shuffle(answers);
      this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);// for storing the index of correct answer
    },
    answeredClass(index){
      let answerClass='';
      answerClass=!this.answered && this.selectedIndex===index?'selected':
                   this.answered && this.correctIndex===index? 'correct':
                   this.answered && this.selectedIndex===index && this.correctIndex!==index ? 'incorrect':'';
      return answerClass;
    }
  }
}
</script>

<style scoped>
  .list-group{
    margin-bottom:15px;
  }
  .list-group-item:hover{
    background: #EEE;
    cursor:pointer;
  }
  .btn{
    margin:0 5px;
  }
  .selected{
    background: lightblue;
    color:whitesmoke;
  }
  .correct{
    background-color: green;
    color:whitesmoke;
  }
  .incorrect{
    background-color:red;
    color: whitesmoke;
  }
</style>