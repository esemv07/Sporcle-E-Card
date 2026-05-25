<script>
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();

	import "$lib/style.css";
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<link rel