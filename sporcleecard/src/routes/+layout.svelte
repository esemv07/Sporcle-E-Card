<script>
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();

	import "$lib/style.css";
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200&icon_names=filter_vintage,keyboard_double_arrow_left,keyboard_double_arrow_right,kid_star,pets" />
</svelte:head>

{@render children()}
