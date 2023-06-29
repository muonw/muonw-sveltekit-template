<script lang="ts">
import { onMount} from "svelte";
import { base } from "$app/paths";

function switchColorScheme(scheme: 'light'|'dark') {
    let html = document.querySelector('html');
    html?.setAttribute('data-color-scheme', scheme);
}

onMount(() => {
    const colorSwitcher = (event: MediaQueryListEvent) => {
        switchColorScheme(event.matches ? 'dark' : 'light');
    }
    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', colorSwitcher);
    return () => {
        window.matchMedia('(prefers-color-scheme: dark)').removeEventListener('change', colorSwitcher);
    }
});

import '$lib/styles/global.scss';
</script>

<nav>
    <div>
        <a href="{base}/">Home</a>
    </div>
    <button class="a compact" id='switch-to-light-mode' title="Light Mode" on:click={()=>switchColorScheme('light')}>☀️</button>
    <button class="a compact" id='switch-to-dark-mode' title="Dark Mode" on:click={()=>switchColorScheme('dark')}>🌙</button>
</nav>

<hr>

<slot />

<style>
nav {
    display: flex;
    justify-content: space-between;
}
#switch-to-light-mode {
    display: var(--dark-mode-display);
    cursor: pointer;
}
#switch-to-dark-mode {
    display: var(--light-mode-display);
    cursor: pointer;
}
</style>