<template>
  <div class="mt-6">
    <p v-if="isCorrect" class="text-green-600 text-lg font-bold">Correct!</p>
    <p v-else class="text-red-600 text-lg font-bold">
      Incorrect! The correct answer is {{ correctAnswer }}.
    </p>
    <button
      @click="$emit('next')"
      class="mt-4 py-2 px-4 bg-blue-500 text-white rounded-md hover:bg-blue-600"
    >
      Next Question
    </button>
  </div>
</template>

<script setup>
defineProps({
  isCorrect: { type: Boolean, required: true },
  correctAnswer: { type: String, required: true },
})
defineEmits(['next'])
</script>
