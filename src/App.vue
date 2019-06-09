<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal"></Header>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <!-- V-if is added to render the questions only after it is fetched from the api -->
          <QuestionBox v-if="questions.length" :currentQuestion="questions[index]" :next="next" :increment="increment"></QuestionBox>
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
    data(){
      return {
        questions:[],
        index:0,
        numCorrect:0,
        numTotal:0
      }
    },
    methods:{
      next:function(){
        this.index++;
      },
      increment(isCorrect){
        if(isCorrect){
          this.numCorrect++;
        }
        this.numTotal++;
      }
    },
    mounted: function(){//it gets called automatically, it is a life cycle function
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
        method:'get'
      }).then(res=>{
        return res.json();
      }).then(jsonData=>{
        this.questions=jsonData.results;//it creates questions as array of questions
      });
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