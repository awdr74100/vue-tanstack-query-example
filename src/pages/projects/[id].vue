<template>
  <h1 class="mt-12 text-center text-4xl text-green-500">Project Page</h1>

  <span v-if="isPending">Loading...</span>
  <span v-else-if="isError">{{ error?.message }}</span>
  <div v-else-if="data">
    <p>Id: {{ data.id }}</p>
    <p>Title: {{ data.title }}</p>
  </div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router/auto';
import { useQuery } from '@tanstack/vue-query';
import type { Project } from '@/types/Project';

const route = useRoute('/projects/[id]');

function fetchProject(id: string): Promise<Project> {
  return fetch(`http://localhost:3000/api/projects/${id}`).then((res) => res.json());
}

const { isPending, isError, data, error } = useQuery({
  queryKey: ['projects', route.params.id],
  queryFn: ({ queryKey }) => fetchProject(queryKey[1]),
});
</script>
