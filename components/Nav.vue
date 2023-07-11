<template>
    <div :class="`component-container ${animationsOn ? 'animations-on' : ''}`">
        <button @click="toggleMenu" :class="`menu-toggle ${!menuOpen ? 'menu-closed' : ''}`">
            <img src="~/assets/arrow.svg" alt="toggle menu" height="40" width="40" />
        </button>
        <aside>
            <div>
                <Transition>
                    <nav v-if="menuOpen">
                        <ul>
                            <li>
                                <div>
                                    Rules
                                </div>
                                <ul>
                                    <li v-for="rule in rules">
                                        <NuxtLink :to="rule._path">{{ rule.title }}</NuxtLink>
                                    </li>
                                </ul>
                            </li>
                            <hr />
                            <li>
                                <NuxtLink to="/playbooks">
                                    Playbooks
                                </NuxtLink>
                                <ContentNav path="/playbooks" />
                            </li>

                            <li>
                                <div class="menu-item-toggle">
                                    <NuxtLink to="/adventures">
                                        Sample adventures
                                    </NuxtLink>
                                </div>
                                <ContentNav path="/adventures" />
                            </li>

                            <li>
                                <NuxtLink to="/inspiration">
                                    Inspiration
                                </NuxtLink>
                            </li>

                            <li>
                                <NuxtLink to="/sheet">
                                    Character sheet
                                </NuxtLink>
                            </li>

                            <li>
                                <NuxtLink to="/bevilacqua-dont-dream-alpha.pdf">
                                    PDF rules
                                </NuxtLink>
                            </li>

                            <li>
                                <NuxtLink to="/contributing">
                                    Contributing
                                </NuxtLink>
                            </li>


                            <!-- <li>
                                <NuxtLink to="/game/new">Create a game</NuxtLink>
                            </li>
                            <li>
                                <NuxtLink to="/sheet/">Create a character</NuxtLink>
                            </li>
                            <li>
                                <NuxtLink to="/account">Login/account</NuxtLink>
                            </li> -->
                        </ul>
                    </nav>
                </Transition>
            </div>
        </aside>

    </div>
</template>

<script setup lang="ts">
const animationsOn = ref(false);
const menuOpen = ref(true);

function toggleMenu() {
    menuOpen.value = !menuOpen.value;
}

onMounted(() => {
    const active = document.querySelector('.router-link-active.router-link-exact-active');
    if (active) {
        active.scrollIntoView({ behavior: 'smooth' });
    }

    animationsOn.value = true;
})

// we need to get all the "rulebook" content
const rules = await queryContent()
    .where(
        {
            rules_page: { $gt: 0 }
        }
    ).find();

// sort it by the key we provide
rules.sort((a, b) => a.rules_page - b.rules_page);

</script>

<style scoped>
.component-container {
    position: relative;
}

.menu-toggle {
    border-radius: 1.5rem;
    display: none;
    top: 10px;
    z-index: 3;
    position: fixed;
}

.animations-on .menu-toggle {
    animation: button-rotator-reverse-mobile 0.5s ease-in-out forwards;
    display: inline;
}

.animations-on .menu-toggle.menu-closed {
    animation: button-rotator-mobile 0.5s ease-in-out forwards;
}

.menu-toggle img {
    filter: invert(1);
}

@media(min-width: 768px) {
    .animations-on .menu-toggle {
        animation: button-rotator-reverse 0.5s ease-in-out forwards;
        top: 150px;
    }

    .animations-on .menu-toggle.menu-closed {
        animation: button-rotator 0.5s ease-in-out forwards;
    }

}


aside {
    padding: 1rem;
    position: fixed;
    top: 75px;
}

aside>div {
    position: relative;
}

nav {
    background: whitesmoke;
    padding: 0.5rem;
    overflow-y: scroll;

    /* fallback in case dvh aren't supported */
    max-height: 82vh;
    max-height: 82dvh;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    margin: 0 0.5rem 1rem;
}

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

/* we will explain what these classes do next! */
.v-enter-active,
.v-leave-active {
    animation: slide-in 0.5s ease-in-out forwards;
}

.v-enter-from,
.v-leave-to {
    animation: slide-out 0.5s ease-in-out forwards;
}

/**
animations

*/
@keyframes button-rotator-reverse {
    0% {
        left: 0;
        transform: rotate(180deg);
    }

    100% {
        left: 230px;
        transform: rotate(0deg);
    }
}

@keyframes button-rotator-reverse-mobile {
    0% {
        right: 10px;
        transform: rotate(180deg);
    }

    100% {
        right: 10px;
        transform: rotate(0deg);
    }
}

@keyframes button-rotator {
    0% {
        left: 230px;
        transform: rotate(0deg);
    }

    100% {
        left: 0;
        transform: rotate(180deg);
    }
}

@keyframes button-rotator-mobile {
    0% {
        right: 10px;
        transform: rotate(0deg);
    }

    100% {
        right: 10px;
        transform: rotate(180deg);
    }
}

@keyframes slide-in {
    0% {
        transform: translateX(-100%);
    }

    100% {
        transform: translateX(0);
    }
}

@keyframes slide-out {
    0% {
        transform: translateX(0);
    }

    100% {
        transform: translateX(-100%);
    }
}
</style>