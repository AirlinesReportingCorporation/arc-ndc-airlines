<script>
    import { slide } from "svelte/transition";
    export let airline
	export let date
    let isOpen = false;
    const toggle = () => isOpen = !isOpen
</script>

<div on:click={toggle} aria-expanded={isOpen}> +
</div>
{#if isOpen}
<div transition:slide={{ duration: 300 }}>
        <p>
          {airline.Designator}
          {airline["Numeric Code"].toString().length == 2
            ? "0" + airline["Numeric Code"]
            : airline["Numeric Code"]}
          {airline["Airline Name"]}
          Latest Updates: {date}
        </p>
</div>
{/if}
