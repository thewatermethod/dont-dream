<template>
    <NuxtLink :class="className" :to="`${doc._path}`">
        {{ doc.title }}
    </NuxtLink>
</template>
    
    
<script setup lang="ts">
defineProps<{
    className: string,
    doc: {
        title: string,
        _path: string,
    },
}>()

</script>