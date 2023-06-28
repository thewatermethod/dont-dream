<template>
    <main>
        <div class="splash">
            <div class="bloody" @mouseenter="moveWords">
                <h1 ref="dont">Don't</h1>
                <h1 ref="dream">Dream</h1>
            </div>
            <nav class="splash-menu">
                <ul>
                    <li>
                        <NuxtLink to="/srd">Rules</NuxtLink>
                    </li>

                    <li>
                        <a href="/bevilacqua-dont-dream-alpha.pdf" download>PDF</a>
                    </li>


                    <!-- <li>
                        <NuxtLink to="/account">Login</NuxtLink>
                    </li> -->
                </ul>
            </nav>
        </div>
    </main>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

useHead({
    title: 'Don\'t Dream',
})

// use a special layout for the home page
definePageMeta({
    layout: "home",
});

// declare a ref to hold the element reference
// the name must match template ref value
const dont = ref(null);
const dream = ref(null);


// this function will be called when the mouse enters the bloody div
// it'll stick the words to the top and bottom of the screen 
function moveWords($event: MouseEvent) {
    const target = $event.target as HTMLElement;
    target.classList.add('up-and-down');
}

onMounted(() => {
    // cast the ref to an HTMLElement
    const dontEl = (dont.value as unknown) as HTMLElement;
    const dreamEl = (dream.value as unknown) as HTMLElement;

    // set the length property on the element
    if (dontEl) {
        dontEl.style.setProperty("--length", String(dontEl.innerText.length));
    }

    if (dreamEl) {
        dreamEl.style.setProperty("--length", String(dreamEl.innerText.length));
    }
});

</script>


<style scoped>
main {
    overflow: hidden;
    max-height: 100vh;
}

.splash {


    overflow: hidden;
    place-items: center;
    position: relative;
    width: 100vw;

    height: 100vh;
    height: 100dvh;
}

@media(min-width: 768px) {
    .splash {
        display: flex;
    }
}


.bloody {
    color: tomato;
    display: flex;
    flex-direction: column;
    place-content: center;
    text-align: center;
}

/* @media(min-width: 768px) {
    .bloody {
        flex-direction: row;
    }
} */

.bloody h1 {
    --width: 100vw;
    --scale: 0.5;
    font-size: calc(var(--width) / (var(--length, 1) * 0.5) * var(--scale, 1));
    margin: 0;
    font-family: "VallejoSerifBlack", cursive;
}

.bloody.up-and-down h1 {
    animation: slideup 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
}

.bloody.up-and-down h1:first-child {
    animation: slidedown 0.7s cubic-bezier(0.455, 0.03, 0.515, 0.955) forwards;
}

@keyframes slidedown {
    0% {
        transform: translateY(0);
    }

    100% {
        transform: translateY(-20px);
    }
}

@keyframes slideup {
    0% {
        transform: translateY(0);
    }

    100% {
        transform: translateY(20px);
    }
}


@media(min-width: 768px) {
    .splash-menu ul {
        gap: 0.5em;
        left: 25%;
        position: absolute;
        top: 50%;
    }

}

.splash-menu ul {
    align-items: center;
    justify-content: center;
    display: flex;
    gap: 1em;
    list-style: none;
    z-index: 0;
}

.splash ul a {
    background: whitesmoke;
    border-radius: 6px;
    color: #333;
    font-size: 1em;
    padding: 0.5rem 1rem;
}

.splash ul a:hover {
    animation: slowred 1s cubic-bezier(0.23, 1, 0.320, 1) forwards;
}

/**
 * todo - this should be a gradient, or several gradients 
 */
@keyframes slowred {
    0% {
        background: whitesmoke;
        color: #333;
    }

    100% {
        background: tomato;
        color: whitesmoke;
    }
}
</style>