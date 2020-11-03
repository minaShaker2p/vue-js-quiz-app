<template>
 <div>

 <div class="question-box-container">
    <div class="jumbotron">
      <template class="lead">
        {{ currentQuestion.question }}
      </template>

       <hr class="my-4">

  <b-list-group>
  <b-list-group-item v-for="(answer,index) in shuffledAnswers
  " :key="index"  @click="selectAnswer(index)"
  :class="answerClass(index)"
  > {{answer}}</b-list-group-item>
</b-list-group>
  
<button type="button" class="btn btn-primary"
@click="submitAnswer"
:disabled="selectedIndex === null || isAnswered"
>Submit</button>
<button type="button" @click="next" class="btn btn-success"
:disabled="numTotal === 10 ">Next</button>
    </div>
 
</div>
 </div>
 
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    numTotal: Number,
    increment :Function
  },
  data() {
    return {
      selectedIndex : null,
      correctIndex : null,
      shuffledAnswers : [] ,
      isAnswered : false
    }
  },
  watch: {
    currentQuestion()
    {
      this.selectedIndex =null
      this.correctIndex =null
      this.isAnswered =false
      this.suffleAnswers()
    }
  },
    methods :{
    selectAnswer(index)
    {
      this.selectedIndex=index
    },
    suffleAnswers()
    {
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers)
      // store the index of the correct answer 
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer()
    {
      let isCorrect =false
      if( this.selectedIndex === this.correctIndex)
      {
        isCorrect = true
      }
      // change statues of question to answered question
      this.isAnswered=true
      this.increment(isCorrect)
    },answerClass(index)
    {
      let answerClass=''
      if(!this.isAnswered && this.selectedIndex === index)
      {
            answerClass='selected'
      }else if (this.isAnswered && this.correctIndex === index)
      {
          answerClass='correct'
      }else if( this.isAnswered && this.correctIndex !== index && this.selectedIndex === index)
      {
          answerClass='incorrect'
      }
      return answerClass
    }
  },
  mounted()
  {
    this.suffleAnswers()
  }
  ,
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }
};
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
  background-color:skyblue
} 
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>