<template>
  <h1 class="mt-12 text-center text-4xl text-green-500">Projects Page</h1>

  <span v-if="isPending">Loading...</span>
  <span v-else-if="isError">Oops: {{ error?.message }}</span>
  <ul v-else-if="data">
    <li v-for="(item, index) in data" :key="index">
      <span>Id: {{ item.id }}</span>
      <span>Title: {{ item.title }}</span>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query';
import type { Project } from '@/types/Project';

function fetchProjects(): Promise<Project[]> {
  return fetch('http://localhost:3000/api/projects').then((res) => res.json());
}

const { isPending, isError, data, error } = useQuery({
  queryKey: ['PROJECTS'],
  queryFn: () => fetchProjects(),
});
</script>
