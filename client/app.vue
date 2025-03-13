<script lang="ts" setup>
import { Toaster } from 'vue-sonner';

useHead({
  titleTemplate: (pageTitle) => {
    return pageTitle
      ? `${pageTitle} - Laravel Nuxt Staterkit`
      : 'Laravel Nuxt Staterkit'
  }
})
</script>

<template>
  <div>
    <Toaster rich-colors position="top-right" />
    <NuxtPage />
  </div>
</template>
