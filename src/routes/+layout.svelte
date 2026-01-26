<script lang="ts">
	// import favicon from '$lib/assets/favicon.svg';
	import Muse from '$lib/Muse.svelte'

	let { children } = $props();
</script>

<svelte:head>
	<script src="/node_modules/plotly.js-dist-min/plotly.min.js"></script>
</svelte:head>

{@render children?.()}
