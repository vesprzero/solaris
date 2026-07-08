<script>
	import favicon from '$lib/assets/logo.png';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>Solari</title>
</svelte:head>

{@render children()}
