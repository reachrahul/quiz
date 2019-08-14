<template>
    <div class="questionbox-container">
      <b-jumbotron >

          <template slot="lead">
            {{CurrentQuestion.question|decoder}}
          </template>

          <hr class="my-4">

          <b-list-group>
            <b-list-group-item 
              v-for="(answer, index) in shuffledAnswers" :key="index"
              @click.prevent="selectanswer(index)" 
              :class="[
              !answered && selectedindex === index ? 'selected' : 
              answered && correctIndex === index ? 'correct' : 
              answered && selectedindex === index &&correctIndex !== index ? 'incorrect' : 
              '' ]"
            >
              {{answer|decoder}}
            </b-list-group-item>
          </b-list-group>

        <b-button class="submit" variant="primary" 
          @click="submitanswer"
          :disabled="selectedindex === null || answered"
        >

        Submit

        </b-button>
        <b-button @click="next" variant="success"  >
          Next
        </b-button>
        
      </b-jumbotron>
   </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    CurrentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedindex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed: {
    answers () {
      let answers=[...this.CurrentQuestion.incorrect_answers]
       answers.push(this.CurrentQuestion.correct_answer)
       return answers
    }
  },
  watch: {
    CurrentQuestion: {
      immediate: true,
      handler () {
        this.selectedindex = null
        this.shuffleAnswers ()
        this.answered = false
      }
    }
    
    //() {
     // this.selectedindex = null
     // this.shuffleAnswers ()
   // }
  },
  filters: {
    decoder(str) {
      var textArea = document.createElement("textarea");
      textArea.innerHTML = str;
      return textArea.value;
    }
  },
  methods : {
    selectanswer (index) {
      this.selectedindex=index    
  },
 submitanswer () {
    let isCorrect = false

    if (this.selectedindex === this.correctIndex) {
      isCorrect = true
    }
    this.answered = true
    this.increment(isCorrect)
  },
  shuffleAnswers () {
    let answers=[...this.CurrentQuestion.incorrect_answers, this.CurrentQuestion.correct_answer]
    this.shuffledAnswers = _.shuffle(answers)
    this.correctIndex = this.shuffledAnswers.indexOf(this.CurrentQuestion.correct_answer)
  },
  
  mounted () {
    this.shuffleAnswers()
  }
}
}
</script>

<style scoped>
  .list-group {
      margin-bottom: 18px;
  }
  .list-group-item:hover {
      background-color: lightgrey;
      cursor: pointer;
  }
  .btn {
      margin-left: 10px;
  }
  .submit:disabled {
      background-color: black;
  }
  .selected {
      background-color: lightskyblue;
  }
  .correct {
      background-color: lightgreen;
  }
  .incorrect {
      background-color: lightcoral;
  }
</style>