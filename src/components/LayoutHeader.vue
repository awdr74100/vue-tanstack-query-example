<template>
  <header>
    <ul class="flex items-center bg-green-500">
      <li v-for="(route, index) in routes" :key="index">
        <RouterLink
          class="inline-block p-4 text-center text-white hover:bg-green-400"
          :to="{ path: route }"
          >{{ route }}</RouterLink
        >
      </li>
    </ul>
  </header>
</template>

<script setup lang="ts">
const routes = [
  '/',
  '/projects',
  '/projects/2',
  '/projects/4',
  '/tests/create',
  '/projects/create',
];
</script>
