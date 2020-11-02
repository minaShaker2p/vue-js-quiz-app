<template>
  <div id="app">
    <Header 
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    />
    <div class="container">
  <div class="row">
    <div class="col-sm-6 offset-3" >
      <QuestionBox 
        :currentQuestion="questions[index]"
        :next="next"
        :increment="increment" />
    </div>
  </div>
  </div>
</div>    
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      questions : [],
      index : 1,
      numCorrect : 0,
      numTotal : 0
    }
  },
  methods : {
    next()
    {
      this.index++
    },
    increment(isCorrect)
    {
      if(isCorrect)
      {
        this.numCorrect++
      }
      this.numTotal++

    }
  }
  ,
  mounted:function()
  {
    fetch('https://opentdb.com/api.php?amount=10&category=21&difficulty=easy&type=multiple',{
      method:'get'
    }).then((response) => {
      return response.json()
    }).then((jsonData) => {
       this.questions = jsonData.results
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
  margin-top: 60px;
}
</style>
