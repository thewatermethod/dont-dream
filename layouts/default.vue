<template>
    <div class="layout-grid">
        <aside>
            <nav>
                <ul>
                    <li><a href="#">Rules</a></li>
                    <li><a href="#">Login</a></li>
                </ul>
            </nav>
        </aside>
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