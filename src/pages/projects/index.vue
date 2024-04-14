<template>
  <h1 class="mt-12 text-center text-4xl text-green-500">Projects Page</h1>

  <span v-if="projectsIsLoading">Loading...</span>
  <span v-else-if="projectsIsError">Oops: {{ projectsError?.message }}</span>
  <ul v-else-if="projects" class="mt-4 border">
    <li v-for="(item, index) in projects" :key="index">
      <RouterLink
        class="inline-block w-full p-4 hover:bg-gray-200"
        :to="{ path: '/projects', query: { pid: item.id } }"
      >
        <span>Id: {{ item.id }}</span>
        <span class="ml-4">Title: {{ item.title }}</span>
      </RouterLink>
    </li>
  </ul>

  <!-- isFetching 是請求發送過程觸發 -->
  <!-- isLoading 是沒有快取時觸發，有快取就不會觸發 -->
  <!-- <span>{{ (isFetching) }} {{ testsIsLoading }}</span> -->
  <span v-if="testsIsLoading">Loading...</span>
  <span v-else-if="testsIsError">Oops: {{ testsError?.message }}</span>

  <ul v-else-if="tests" class="mt-4 border">
    <li v-for="(item, index) in tests" :key="index" class="w-full p-4 hover:bg-gray-200">
      <span class="">Id: {{ item.id }}</span>
      <span class="ml-4">Title: {{ item.title }}</span>
      <span class="ml-4">Status: {{ item.status }}</span>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query';
import type { Project } from '@/types/Project';
import type { Test } from '@/types/Test';
import { useRoute } from 'vue-router/auto';
import { computed } from 'vue';

function fetchProjects(): Promise<Project[]> {
  return fetch('http://localhost:3000/api/projects').then((res) => res.json());
}

const {
  isPending: projectsIsLoading,
  isError: projectsIsError,
  data: projects,
  error: projectsError,
} = useQuery({
  queryKey: ['projects'],
  queryFn: () => fetchProjects(),
});

function fetchTestsByProject(id: string | undefined): Promise<Test[]> {
  return fetch(`http://localhost:3000/api/tests?pid=${id}`).then((res) => res.json());
}

const route = useRoute('/projects/');
const projectId = computed(() => {
  return (route.query.pid as string | undefined) ?? projects.value?.[0]?.id;
});
const enabled = computed(() => {
  return !!projects.value?.find(({ id }) => id === projectId.value);
});

const {
  isPending: testsIsLoading,
  isError: testsIsError,
  // isFetching,
  data: tests,
  error: testsError,
} = useQuery({
  queryKey: ['tests', projectId],
  queryFn: () => fetchTestsByProject(projectId.value),
  enabled,
  // staleTime:Infinity
});
</script>
