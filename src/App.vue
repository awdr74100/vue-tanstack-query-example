<template>
  <div class="mx-auto min-h-screen max-w-5xl">
    <LayoutHeader />
    <RouterView />
  </div>
</template>

<script setup lang="ts">
import LayoutHeader from '@/components/LayoutHeader.vue';
</script>
