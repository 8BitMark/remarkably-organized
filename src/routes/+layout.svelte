<script lang="ts">
	import { dev } from '$app/environment';
	import '$lib/styles/global.scss';
</script>

<svelte:head>
	<title>Remarkably Organized</title>
</svelte:head>
<slot />

<style lang="scss">
	:global(body > *:not(#root)) {
		@media print {
			display: none !important;
		}
	}
</style>
