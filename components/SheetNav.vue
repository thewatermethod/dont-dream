<template>
    <nav class="sheet-nav">
        <ul>
            <li>
                <NuxtLink to="/sheet/">Info & Attributes</NuxtLink>
            </li>
            <li>
                <NuxtLink to="/sheet/moves">Playbook & Moves</NuxtLink>
            </li>
            <li>
                <NuxtLink to="/sheet/bonds">Bonds</NuxtLink>
            </li>
        </ul>
    </nav>
</template>

<style>
.sheet-nav {
    padding: 0 0 1.5rem;
}

.sheet-nav ul {
    display: flex;
    justify-content: space-between;
    max-width: 680px;
}

.sheet-nav li {
    border-top: 1px solid tomato;
    border-left: 1px solid tomato;
    border-right: 1px solid tomato;
    border-radius: 0.25rem 0.25rem 0 0;
    list-style: none;
    min-width: 200px;
    padding: 0.5rem 1rem;
}

.sheet-nav li a {
    color: tomato;
    text-decoration: none;
    text-transform: uppercase;
}

.sheet-nav li a:hover {
    text-decoration: underline;
    text-decoration-color: darkblue;
}

.sheet-nav li:has([aria-current]) {
    background: tomato;
}

.sheet-nav li:has([aria-current]) a {
    color: white;
}
</style>

