<template>
  <div class="flex flex-col gap-3">
    <Option
      v-for="(option, index) in options"
      :key="index"
      :option="option"
      :disabled="answerSelected"
      :is-correct="answerSelected && option === correctAnswer"
      :is-selected="answerSelected && option === selectedAnswer"
      @click="emit('select-answer', option)"
    />
  </div>
</template>

<script setup>
import Option from './Option.vue'
const emit = defineEmits(['select-answer'])
defineProps({
  options: { type: Array, required: true },
  answerSelected: { type: Boolean, required: true },
  selectedAnswer: { type: String, required: true },
  correctAnswer: { type: String, required: true }
})
</script>