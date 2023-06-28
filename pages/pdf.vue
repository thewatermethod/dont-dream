<template>
    <div>
        <div v-for="page in pages" class="page">
            <h2>{{ page.title }}</h2>
            <ContentRenderer :value="page" />
            <div v-if="page._dir === 'playbooks'">
                <ContentQuery v-if="page._path" v-slot="{ data }" :path="`${page._path.replace('playbooks', 'moves')}`">
                    <div v-for="move in data" class="playbook-move">
                        <h3> {{ move.title }}</h3>
                        <ContentRenderer :value="move" />
                    </div>
                </ContentQuery>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">

useHead({
    title: 'Don\'t Dream',
});

const data = await queryContent('/').find();
const pages = data.filter((p) => p.pdf_order)
    .sort((a, b) => a.pdf_order - b.pdf_order);

</script>
<style>
@media print {
    .page {
        page-break-inside: avoid;
        margin: 1em auto 5em;
        max-width: 480px;
    }

    .layout-grid {
        display: block !important;
    }

    .site-title {
        background: transparent !important;
        position: relative !important;
        color: #333 !important;
    }

    .site-title h1 {
        color: #333 !important;
        font-size: 6em !important;
    }

    aside,
    nav,
    button {
        display: none !important;
    }
}
</style>