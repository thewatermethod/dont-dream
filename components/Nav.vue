<template>
    <aside>
        <h1>Don't Dream</h1>
        <nav>
            <ul>
                <li>
                    <NuxtLink to="/srd">Rules</NuxtLink>
                </li>
                <li>
                    <NuxtLink to="/sheet">Create a character</NuxtLink>
                </li>
                <li>
                    <NuxtLink to="/account">Login/account</NuxtLink>
                </li>
            </ul>
        </nav>
    </aside>
</template>

<style scoped>
h1 {
    color: tomato;
    font-family: 'Permanent Marker', cursive;
    margin: 0;
}

aside {
    padding: 1rem;
    background: whitesmoke;
    min-height: 100vh;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    margin: 0 0.5rem 1rem;
}

a,
a:visited {
    color: darkblue;
}
</style>