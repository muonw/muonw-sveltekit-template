<script lang="ts">
import { onMount} from "svelte";
import { base } from "$app/paths";
import { pageTitle } from '$lib/stores/mainStore';
import { PUBLIC_WEBSITE_TITLE } from "$env/static/public";

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

<svelte:head>
    <title>{$pageTitle ? $pageTitle + ' - ' : ''}{PUBLIC_WEBSITE_TITLE}</title>
</svelte:head>

<div id="top-bar" class="contained">
    <nav>
        <div>
            <a href="{base}/">Home</a>
        </div>
        <div>
            <button class="a compact" id='switch-to-light-mode' title="Light Mode" on:click={()=>switchColorScheme('light')}>☀️</button>
            <button class="a compact" id='switch-to-dark-mode' title="Dark Mode" on:click={()=>switchColorScheme('dark')}>🌙</button>
        </div>
    </nav>
</div>

<slot />

<style>
:global(body){
    margin: 0;
}
#top-bar {
    position: sticky;
    top: 0;
    background-color: var(--colors-body-background);
    border-bottom: 1px solid var(--colors-hr);
    box-shadow: 0px 3px 2px -2px var(--colors-hr);
    z-index: 11;
}
nav {
    display: flex;
    justify-content: space-between;
    padding: .3rem 0 .3rem 0;
    margin: auto;
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