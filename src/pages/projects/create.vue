<template>
  <h1 class="mt-12 text-center text-4xl text-green-500">Project Create Page</h1>
  <form @submit.prevent="mutate({ id, title })">
    <div>
      <label for="testId">Id: </label>
      <input id="testId" v-model="id" type="text" class="border border-green-500" />
    </div>
    <div class="mt-4">
      <label for="title">Title: </label>
      <input id="title" v-model="title" type="text" class="border border-green-500" />
    </div>
    <div class="mt-4">
      <button type="submit" class="rounded bg-green-500 p-4 text-white hover:bg-green-400">
        Submit
      </button>
    </div>
  </form>

  <div class="mt-4">
    <span v-if="isPending">Loading...</span>
    <span v-else-if="isError">Oops: {{ error?.message }}</span>
    <span v-else-if="isSuccess">Project added!</span>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import type { Project } from '@/types/Project';
import { useMutation, useQueryClient } from '@tanstack/vue-query';

const id = ref('');
const title = ref('');

function createProject(project: Project): Promise<Project> {
  return fetch('http://localhost:3000/api/projects', {
    method: 'post',
    body: JSON.stringify(project),
    headers: {
      'Content-Type': 'application/json',
    },
  }).then((res) => res.json());
}

const queryClient = useQueryClient();

const { isPending, isError, isSuccess, error, mutate } = useMutation({
  mutationFn: (newProject: Project) => createProject(newProject),
  onSuccess(newProject) {
    queryClient.setQueryData(['projects', newProject.id], newProject);
    // queryClient.invalidateQueries({ queryKey: ['projects'] });
  },
});
</script>
