<template>
  <p class="text-xl font-semibold mb-4">Score: {{ score }} / {{ totalQuestions }}</p>
</template>

<script setup>
defineProps({
  score: { type: Number, required: true },
  totalQuestions: { type: Number, required: true },
})
</script>
