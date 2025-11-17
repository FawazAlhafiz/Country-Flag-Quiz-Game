<template>
  <button
    @click="$emit('click')"
    :disabled="disabled"
    :class="[
      'py-3 px-6 text-lg rounded-md transition-colors',
      disabled ? 'cursor-not-allowed' : 'cursor-pointer',
      isCorrect ? 'bg-green-500 text-white' : '',
      isSelected && !isCorrect ? 'bg-red-500 text-white' : '',
      !disabled && !isCorrect && !isSelected ? 'bg-blue-100 hover:bg-blue-200' : ''
    ]"
  >
    {{ option }}
  </button>
</template>

<script setup>
defineProps({
  option: { type: String, required: true },
  disabled: { type: Boolean, default: false },
  isCorrect: { type: Boolean, default: false },
  isSelected: { type: Boolean, default: false }
})
defineEmits(['click'])
</script>