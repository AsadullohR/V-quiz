<script setup>
import {ref, computed} from 'vue'

const questions = ref([
{
	question: 'What is Vue?',
	answer: 0,
	options: [
		'A framework',
		'A library',
		'A type of hat'
	],
	selected: null
  },
  {
	question: 'What is Vuex used for?',
	answer: 2,
	options: [
		'Eating a delicious snack',
		'Viewing things',
		'State management'
	],
	selected: null
  },
  {
	question: 'What is Vue Router?',
	answer: 1,
	options: [
		'An ice cream maker',
		'A routing library for Vue',
		'Burger sauce'
	],
	selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(()=> {
  let value = 0
  questions.value.map(q => {
    if(q.selected == q.answer){
      value++
    }
  })
  return value
})
const getCurrentQuestion = computed(() =>{
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const setAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
}

const nextQuestion = () => {
  if(currentQuestion.value < questions.value.length - 1){
      currentQuestion.value++
  }else{
    quizCompleted.value = true
  }
}


</script>

<template>
<main class="app">
  <h1>The Quiz</h1>

  <section class="quiz" v-if="!quizCompleted">
    <div class="quiz-info">
      <span class="question">{{ getCurrentQuestion.question }}</span>
      <span class="score">Score {{ score }}/{{ questions.length }}</span>
    </div>
    <div class="options">
        <label 
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
            ? index == getCurrentQuestion.answer
              ? 'correct'
              : 'wrong'
            : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`">
        
        <input
          type="radio"
          :name="getCurrentQuestion.index"
          :value="index"
          v-model="getCurrentQuestion.selected"
          :disabled="getCurrentQuestion.selected"
          @change="setAnswer"
        >
        <span>{{ option }}</span>
      </label>
    </div>

    <button
      @click="nextQuestion"
      :disabled="!getCurrentQuestion.selected">
      {{ 
        getCurrentQuestion.index == questions.length - 1
          ? 'Finish'
          : getCurrentQuestion.selected == null
            ? 'Select an option'
            : 'Next Question'
       }}
    </button>
  </section>

  <section v-else>
    <h3>You have finished your quiz!</h3>
    <p>Your score is {{ score }}/{{questions.length}}</p>
  </section>
</main>
</template>

<style scoped>


</style>
