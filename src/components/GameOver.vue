<template>
  <div class="mt-6">
    <p class="text-xl font-bold">Game Over! Final Score: {{ score }} / {{ totalQuestions }}</p>
    <button @click="$emit('reset')" class="mt-4 py-2 px-4 bg-green-500 text-white rounded-md hover:bg-green-600">Play Again</button>
  </div>
</template>

<script setup>
defineProps({
  score: { type: Number, required: true },
  totalQuestions: { type: Number, required: true }
})
defineEmits(['reset'])
</script>