<template>
    <div class="layout-grid">
        <Nav />
        <main class="main-content">
            <slot />
        </main>
    </div>
</template>

<style>
.main-content {
    padding: 0 1rem;
}

.layout-grid {
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-template-rows: 1fr;
}
</style>