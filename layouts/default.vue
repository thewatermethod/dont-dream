<template>
    <div class="layout-grid">
        <Nav />
        <slot />
    </div>
</template>

<style>
.layout-grid {
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-template-rows: 1fr;
}
</style>