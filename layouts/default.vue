<template>
    <div class="site-title">
        <h1>
            Don't Dream
        </h1>
        <span>alpha playtest version</span>
    </div>
    <div class="layout-grid">
        <Nav />
        <main class="main-content">
            <slot />
        </main>
    </div>
</template>

<script setup lang="ts">

useHead({
    link: [
        {
            rel: 'preconnect',
            href: 'https://fonts.googleapis.com'
        },
        {
            rel: 'preconnect',
            href: 'https://fonts.gstatic.com',
        },
        {
            href: 'https://fonts.googleapis.com/css2?family=Vollkorn:ital,wght@0,400;0,700;1,400;1,700&display=swap',
            rel: 'stylesheet'
        },
    ],
    title: 'My App',
    meta: [
        { name: 'description', content: 'My amazing site.' }
    ],
    bodyAttrs: {
        class: 'test'
    },
    script: [{ innerHTML: 'console.log(\'Hello world\')' }]
})
</script>

<style>
* {
    font-family: 'Vollkorn', serif;
}

.callout-box {
    border: 1px solid #333;
    box-shadow: 1px 1px 1px tomato;
    padding: 1rem;
    border-radius: 0.5rem;
    margin: 1rem 0;
}

.main-content {
    padding: 0 1rem;
}

.content {
    max-width: 540px;
}

.site-title {
    background: #333;
    color: white;
    text-align: center;
    position: sticky;
    top: 0;
    z-index: 2;
}

.site-title h1 {
    color: tomato;
    font-family: 'Permanent Marker', cursive;
    margin: 0;
}

.layout-grid {
    position: relative;
}

@media(min-width: 1100px) {
    .layout-grid {
        display: grid;
        grid-template-columns: 300px 900px;
        grid-template-rows: 1fr;
        min-height: 100vh;
    }
}
</style>