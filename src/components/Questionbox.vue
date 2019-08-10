<template>
    <div class="questionbox-container">
      <b-jumbotron >

          <template slot="lead">
            {{CurrentQuestion.question|decoder}}
          </template>

          <hr class="my-4">

          <b-list-group>
            <b-list-group-item 
              v-for="(answer, index) in answers" :key="index"
              @click.prevent="selectanswer(index)" 
              :class="[selectedindex === index ? 'selected' : '']"
            >
              {{answer|decoder}}
            </b-list-group-item>
          </b-list-group>

        <b-button variant="primary" 
          @click="submitanswer"
        >

        Submit
        
        </b-button>
        <b-button @click="next" variant="success" href="#" >
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
      shuffledAnswers: []
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
  shuffleAnswers () {
    let answers=[...this.CurrentQuestion.incorrect_answers, this.CurrentQuestion.correct_answer]
    this.shuffledAnswers = _.shuffle(answers)
  },
  submitanswer () {
    let isCorrect = false

    if (this.selectedindex === correctindex) {
      isCorrect = true
    }
    this.increment(isCorrect)
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
  .selected {
      background-color: aquamarine;
  }
  .correct {
      background-color: lightgreen;
  }
  .incorrect {
      background-color: lightcoral;
  }
</style>