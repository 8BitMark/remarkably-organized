<script lang="ts">
	import { PlannerSettings, intersect, type Week } from '$lib';
	import Page from '$lib/components/Page.svelte';
	import SideNav from './SideNav.svelte';
	import TopNav from './TopNav.svelte';

	let { week = {} as Week, settings = {} as PlannerSettings } = $props();
</script>

<article id={week.id} use:intersect={{ rootMargin: '1000px 0px 1000px 0px' }}>
	<SideNav tabs={settings.weekPage.sideNavDisplay} {settings} timeframe={week}></SideNav>
	<TopNav {settings} timeframe={week} />
	<Page {settings} display={settings.weekPage.template} timeframe={week} />
</article>



<style lang="scss">
	article {
		display: flex;
		align-items: center;
		flex-direction: column;
		padding-left: var(--sidenav-width);
		padding-top: var(--topnav-height);
	}
	:global(main.side-nav-right) article {
		padding-right: var(--sidenav-width);
		padding-left: 0;
	}
</style>
