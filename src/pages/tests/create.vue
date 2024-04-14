<template>
  <h1 class="mt-12 text-center text-4xl text-green-500">Test Create Page</h1>
  <form @submit.prevent="mutate({ id, title, projectId, status })">
    <div>
      <label for="testId">Id: </label>
      <input id="testId" v-model="id" type="text" class="border border-green-500" />
    </div>
    <div class="mt-4">
      <label for="projectId">ProjectId: </label>
      <input id="projectId" v-model="projectId" type="text" class="border border-green-500" />
    </div>
    <div class="mt-4">
      <label for="title">Title: </label>
      <input id="title" v-model="title" type="text" class="border border-green-500" />
    </div>
    <div class="mt-4">
      <label for="status">status: </label>
      <input id="status" v-model.number="status" type="text" class="border border-green-500" />
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
    <span v-else-if="isSuccess">Test added!</span>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import type { Test } from '@/types/Test';
import { useMutation, useQueryClient } from '@tanstack/vue-query';

const id = ref('');
const projectId = ref('');
const title = ref('');
const status = ref(0);

function createTest(test: Test): Promise<Test> {
  return fetch('http://localhost:3000/api/tests', {
    method: 'post',
    body: JSON.stringify(test),
    headers: {
      'Content-Type': 'application/json',
    },
  }).then((res) => res.json());
}

const queryClient = useQueryClient();

const { isPending, isError, isSuccess, error, mutate } = useMutation({
  mutationFn: (newTest: Test) => createTest(newTest),
  onSuccess(newTest) {
    queryClient.setQueryData(['tests', newTest.id], newTest);
  },
});
</script>
