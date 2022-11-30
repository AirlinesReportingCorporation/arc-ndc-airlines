<script>
    import { slide } from "svelte/transition";
    // export let airline
	// export let date
    let isOpen = false;
    const toggle = () => isOpen = !isOpen
</script>

<div on:click={toggle} aria-expanded={isOpen}> +
</div>
{#if isOpen}
<div transition:slide={{ duration: 300 }}>I'm code for when you click the + !</div>
{/if}
