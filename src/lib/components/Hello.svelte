<script>
    let {name} = $props();
</script>
{#if name === 'world'}
 <h1>Hello {name}!</h1>
{:else}
 <h1>I don't say hello to anyone else</h1>
{/if}
<!-- <h1>Hello {name}!</h1> -->