<template>
  <ul class="flex items-center gap-4">
    <li>
      <button
        :disabled="model === 1"
        class="text-green-500 disabled:cursor-not-allowed disabled:text-green-200"
        @click="model -= 1"
      >
        prev
      </button>
    </li>
    <li v-for="item in length" :key="item">
      <button
        class="rounded-full bg-green-500 px-4 py-2 text-white"
        :class="{ 'bg-green-700': model === item }"
        @click="model = item"
      >
        {{ item }}
      </button>
    </li>
    <li>
      <button
        :disabled="model === length"
        class="text-green-500 disabled:cursor-not-allowed disabled:text-green-200"
        @click="model += 1"
      >
        next
      </button>
    </li>
  </ul>
</template>

<script setup lang="ts">
defineProps<{
  length: number;
}>();

const model = defineModel<number>({ required: true });
</script>
