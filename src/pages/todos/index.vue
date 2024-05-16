<template>
  <h1>Tests</h1>
  <button class="border border-black" @click="enabled = !enabled">enabled = {{ enabled }}</button>
  <span>{{ isFetching }}</span>
  <span>{{ isLoading }}</span>
  <pre>{{ tests }}</pre>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuery } from '@tanstack/vue-query';
import type { Test } from '@/types/Test';

function fetchTests(): Promise<Test[]> {
  return fetch(`http://localhost:3000/api/todos`).then((res) => res.json());
}

const enabled = ref(false);

const {
  data: tests,
  isFetching,
  isLoading,
} = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetchTests(),
  staleTime: 0,
  enabled,
});
</script>
