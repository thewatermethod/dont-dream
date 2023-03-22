<template>
    <article class="move-card">
        <h3>{{ move.title }}</h3>
        <ContentRendererMarkdown :value="move" />
    </article>
</template>

<script setup lang="ts">
defineProps<{
    move: Move
}>()
</script>

<style scoped>
article {
    box-shadow: 1px 1px 1px white;
}
</style>