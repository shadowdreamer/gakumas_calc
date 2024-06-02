<template lang="pug">
slot
</template>
<script setup lang="ts">
const route = useRoute()

useHead({
  title: `${route.meta.title || '学mas'}`,
  meta: [{ property: 'og:title', content: `App Name - ${route.meta.title}` }]
})
</script>