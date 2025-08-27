<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.png';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>RootZero</title>
	<meta name="description" content="A simple web app to find roots of equations using numerical methods" />
	<meta name="viewport" content="width=device-width, initial-scale=1" />
</svelte:head>

{@render children?.()}