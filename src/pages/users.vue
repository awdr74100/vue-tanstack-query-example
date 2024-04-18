<template>
  <div>
    <AppPagination v-model="page" :length="11" class="my-12" />
    <span v-if="isPending">Loading...</span>
    <span v-else-if="isError">{{ error?.message }}</span>
    <ul v-else-if="users">
      <li v-for="item in users" :key="item.id">
        <span>ID: {{ item.id }}</span>
        <span>NAME: {{ item.name }}</span>
        <span>EMAIL: {{ item.email }}</span>
        <span>SEX: {{ item.sex }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import AppPagination from '@/components/AppPagination.vue';
import { ref, watch } from 'vue';
import type { User } from '@/types/User';
import { useQuery, keepPreviousData } from '@tanstack/vue-query';

const page = ref(1);

function fetchUsers(page: string | number): Promise<User[]> {
  return fetch(`http://localhost:3000/api/users?page=${page}`).then((res) => res.json());
}

const {
  isPending,
  isError,
  data: users,
  error,
  isSuccess,
} = useQuery({
  queryKey: ['users', page],
  queryFn: ({ queryKey }) => fetchUsers(queryKey[1]),
  placeholderData: keepPreviousData,
});

watch(isSuccess, (_, newState) => {
  console.log(newState);
});
</script>
