<template>
  <div class="quiz-container">
    <h1>Country Flag Quiz</h1>
    <p>Score: {{ score }} / {{ totalQuestions }}</p>
    <div v-if="isLoading">Loading countries...</div>
    <div v-else-if="currentQuestion">
      <img :src="currentQuestion.flag" alt="Country Flag" class="flag" />
      <p>What country does this flag belong to?</p>
      <div class="options">
        <button
          v-for="(option, index) in currentQuestion.options"
          :key="index"
          @click="checkAnswer(option)"
          :disabled="answerSelected"
          :class="{
            correct: answerSelected && option === currentQuestion.country,
            incorrect:
              answerSelected && option !== currentQuestion.country && selectedAnswer === option,
          }"
        >
          {{ option }}
        </button>
      </div>
      <div v-if="answerSelected">
        <p v-if="selectedAnswer === currentQuestion.country">Correct!</p>
        <p v-else>Incorrect! The correct answer is {{ currentQuestion.country }}.</p>
        <button @click="nextQuestion">Next Question</button>
      </div>
    </div>
    <div v-else>
      <p>Game Over! Final Score: {{ score }} / {{ totalQuestions }}</p>
      <button @click="resetGame">Play Again</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const countries = ref([])
const isLoading = ref(true)
const currentQuestion = ref(null)
const score = ref(0)
const questionCount = ref(0)
const totalQuestions = 5 // Change this to adjust game length
const answerSelected = ref(false)
const selectedAnswer = ref('')

async function fetchCountries() {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all?fields=name,flags')
    const data = await response.json()
    countries.value = data.map((country) => ({
      name: country.name.common,
      flag: country.flags.png, // or country.flags.svg for vector
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

  // Pick a random correct country
  const randomIndex = Math.floor(Math.random() * countries.value.length)
  const correctCountry = countries.value[randomIndex]

  // Generate 3 random distractors (ensure no duplicates)
  const options = [correctCountry.name]
  while (options.length < 4) {
    const distractorIndex = Math.floor(Math.random() * countries.value.length)
    const distractor = countries.value[distractorIndex].name
    if (!options.includes(distractor)) {
      options.push(distractor)
    }
  }

  // Shuffle options
  options.sort(() => Math.random() - 0.5)

  currentQuestion.value = {
    country: correctCountry.name,
    flag: correctCountry.flag,
    options,
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

<style>
.quiz-container {
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

.flag {
  width: 300px;
  height: auto;
  margin-bottom: 20px;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}

.correct {
  background-color: green;
  color: white;
}

.incorrect {
  background-color: red;
  color: white;
}
</style>
