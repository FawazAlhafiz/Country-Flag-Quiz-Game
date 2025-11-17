<template>
  <img :src="src" alt="Country Flag" class="w-72 h-auto mx-auto mb-6 rounded shadow-md" />
</template>

<script setup>
defineProps({
  src: { type: String, required: true }
})
</script>