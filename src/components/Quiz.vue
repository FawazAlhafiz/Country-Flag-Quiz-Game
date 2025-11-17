<template>
  <div class="max-w-2xl mx-auto p-4 text-center font-sans">
    <h1 class="text-3xl font-bold mb-4">Country Flag Quiz</h1>
    <Score :score="score" :total-questions="totalQuestions" />
    <div v-if="isLoading" class="text-lg">Loading countries...</div>
    <div v-else-if="currentQuestion">
      <Flag :src="currentQuestion.flag" />
      <p class="text-xl mb-4">What country does this flag belong to?</p>
      <Options
        :options="currentQuestion.options"
        :answer-selected="answerSelected"
        :selected-answer="selectedAnswer"
        :correct-answer="currentQuestion.country"
        @select-answer="checkAnswer"
      />
      <Feedback
        v-if="answerSelected"
        :is-correct="selectedAnswer === currentQuestion.country"
        :correct-answer="currentQuestion.country"
        @next="nextQuestion"
      />
    </div>
    <GameOver
      v-else
      :score="score"
      :total-questions="totalQuestions"
      @reset="resetGame"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Score from './Score.vue'
import Flag from './Flag.vue'
import Options from './Options.vue'
import Feedback from './Feedback.vue'
import GameOver from './GameOver.vue'

const countries = ref([])
const isLoading = ref(true)
const currentQuestion = ref(null)
const score = ref(0)
const questionCount = ref(0)
const totalQuestions = 10
const answerSelected = ref(false)
const selectedAnswer = ref('')

async function fetchCountries() {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all?fields=name,flags')
    const data = await response.json()
    countries.value = data.map(country => ({
      name: country.name.common,
      flag: country.flags.png
    }))
    isLoading.value = false
    generateQuestion()
  } catch (error) {
    console.error('Error fetching countries:', error)
    isLoading.value = false
  }
}

function generateQuestion() {
  if (questionCount.value >= totalQuestions) {
    currentQuestion.value = null
    return
  }

  const randomIndex = Math.floor(Math.random() * countries.value.length)
  const correctCountry = countries.value[randomIndex]

  const options = [correctCountry.name]
  while (options.length < 4) {
    const distractorIndex = Math.floor(Math.random() * countries.value.length)
    const distractor = countries.value[distractorIndex].name
    if (!options.includes(distractor)) {
      options.push(distractor)
    }
  }

  options.sort(() => Math.random() - 0.5)

  currentQuestion.value = {
    country: correctCountry.name,
    flag: correctCountry.flag,
    options
  }

  answerSelected.value = false
  selectedAnswer.value = ''
  questionCount.value++
}

function checkAnswer(option) {
  if (answerSelected.value) return
  selectedAnswer.value = option
  answerSelected.value = true
  if (option === currentQuestion.value.country) {
    score.value++
  }
}

function nextQuestion() {
  generateQuestion()
}

function resetGame() {
  score.value = 0
  questionCount.value = 0
  generateQuestion()
}

onMounted(fetchCountries)
</script>