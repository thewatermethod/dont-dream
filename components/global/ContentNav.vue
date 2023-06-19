<template>
    <ul>
        <li v-for="content in contents">
            <NuxtLink :to="`${content._path}`">
                {{ content.title }}
            </NuxtLink>
        </li>
    </ul>
</template>

<script setup lang="ts">
const props = defineProps(['path']);

const contents = await queryContent(props.path)
    .where(
        {
            _path: { $ne: props.path },
        }
    ).find();
</script>

<style scoped>
/**
    Nested menu/list styles
*/
li:has(ul) {
    font-weight: bold;

}

li>ul {
    font-weight: normal;
}

li>ul li {
    margin: 0 0.5rem;
}
</style>