<script lang="ts">
import { onMount} from "svelte";

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
    <span id='switch-to-light-mode' title="Light Mode" on:click={()=>switchColorScheme('light')} on:keypress={()=>switchColorScheme('light')}>☀️</span>
    <span id='switch-to-dark-mode' title="Dark Mode" on:click={()=>switchColorScheme('dark')} on:keypress={()=>switchColorScheme('dark')}>🌙</span>
</nav>

<hr>

<slot />

<style>
#switch-to-light-mode {
    display: var(--dark-mode-display);
    cursor: pointer;
}

#switch-to-dark-mode {
    display: var(--light-mode-display);
    cursor: pointer;
}
</style>