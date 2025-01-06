<script lang="ts">
	import { formatToString, PlannerSettings, type Timeframe } from '$lib';
	import HomeIcon from '~icons/material-symbols-light/calendar-month';
	import { getFontInfo } from '../fonts/fonts';

	let {
		timeframe = {} as Timeframe,
		settings = {} as PlannerSettings,
		breadcrumbs = [] as { name: string; href: string }[],
         	tabs = 'month' as
			| 'days-this-week'
			| 'days-this-month'
			| 'days-this-year'
			| 'weeks-this-year'
			| 'weeks-this-month'
			| 'months'
			| 'quarters'
			| 'years'
			| 'none',
		numWeeksInSideNav = 15,
		numDaysInSideNav = 15,
		disableActiveIndicator = false,
	} = $props();

	const showYearBreadcrumb = false; //$derived(!settings.yearPage.disable && timeframe.year);
	const showQuarterBreadcrumb = $derived(
		!settings.quarterPage.disable && timeframe.year && timeframe.quarter,
	);
	const showMonthBreadcrumb = $derived(
		!settings.monthPage.disable && timeframe.year && timeframe.month,
	);
	const showWeekBreadcrumb = $derived(
		!settings.weekPage.disable &&
			timeframe.year &&
			timeframe.month &&
			timeframe.weekSinceYear,
	);
	const showDayBreadcrumb = $derived(
		!settings.dayPage.disable &&
			timeframe.year &&
			timeframe.month &&
			timeframe.daySinceMonth,
	);
	const isFinalMonth = $derived(
		settings.months.findIndex(
			(m) =>
				m.year === timeframe.start.getUTCFullYear() &&
				m.month === timeframe.start.getUTCMonth() + 1,
		) ===
			settings.months.length - 1,
	);
	const isFinalWeek = $derived(
		settings.weeks.findIndex((m) => m.start.getTime() === timeframe.start.getTime()) ===
			settings.months.length - 1,
	);
	const year = $derived(
		isFinalMonth || isFinalWeek || !timeframe.year
			? timeframe.start.getUTCFullYear()
			: timeframe.year,
	);
	const month = $derived(
		isFinalMonth || isFinalWeek || !timeframe.month
			? timeframe.start.getUTCMonth() + 1
			: timeframe.month,
	);
	const quarter = $derived(Math.floor((month - 1) / 3) + 1);

	const font = $derived(settings.topNav.font);
	const homeIconAdjustments = new Map([
		['Abril Fatface', '-.25rem'],
		['Bebas Neue', '-.4rem'],
		['Acme', '-.15em'],
		['Anton', '-.13em'],
		['Indie Flower', '-.3em'],
		['Just Another Hand', '-.2em'],
		['Lilita One', '-.07em'],
		['Lobster', '-.07em'],
		['Montserrat', '-.09em'],
		['Permanent Marker', '-.09em'],
		['Playfair Display', '.03em'],
		['Poppins', '-.16em'],
		['Rancho', '-.12em'],
		['Roboto', '-.16em'],
		['Roboto Condensed', '-.14rem'],
		['Roboto Slab', '-.1em'],
		['Satisfy', '-.27em'],
		['Shadows Into Light Two', '-.15em'],
	]);
	const navHeightAdjustments = new Map([
		['Abril Fatface', '-.35rem'],
		['Acme', '-.25rem'],
		['Anton', '-.15rem'],
		['Caveat', '-.25rem'],
		['Caveat Brush', '-.25rem'],
		['Dancing Script', '-.25rem'],
		['DM Serif Display', '-.25rem'],
		['Lobster', '-.15rem'],
		['Pacifico', '-.2rem'],
		['Permanent Marker', '-.4rem'],
		['Playfair Display', '-.25rem'],
		['PT Serif', '-.2rem'],
		['Roboto', '-.15rem'],
		['Roboto Condensed', '-.25rem'],
		['Roboto Slab', '-.15rem'],
	]);

	const weekList = $derived(
		settings.weeks.filter(
			(week, i) =>
				week.year === timeframe.year &&
				(tabs === 'weeks-this-year' || week.month === timeframe.month) &&
				(tabs !== 'weeks-this-year' ||
					settings.weeks[i - 1]?.weekSinceYear !== week.weekSinceYear),
		),
	);
	const weekListActiveIndex = $derived(
		weekList.findIndex((week) => week.weekSinceYear === timeframe.weekSinceYear),
	);
	const startWeek = $derived(
		Math.min(
			weekList.length - numWeeksInSideNav,
			Math.ceil(Math.max(0, weekListActiveIndex - numWeeksInSideNav / 2)),
		),
	);
	const weeks = $derived(weekList.slice(startWeek, startWeek + numWeeksInSideNav));

	const dayList = $derived(
		settings.days.filter((day) =>
			tabs === 'days-this-week'
				? day.weekYear === (timeframe.weekYear || timeframe.year) &&
					day.weekSinceYear === timeframe.weekSinceYear
				: tabs === 'days-this-month'
					? day.year === year && month === day.month
					: day.year === year,
		),
	);
	const startDay = $derived(
		Math.min(
			dayList.length - numDaysInSideNav,
			Math.ceil(
				Math.max(
					0,
					(tabs === 'days-this-year'
						? timeframe.daySinceYear || 0
						: tabs === 'days-this-month'
							? timeframe.daySinceMonth || 0
							: timeframe.daySinceWeek || 0) -
						numDaysInSideNav / 2,
				),
			),
		),
	);
	const days = $derived(dayList.slice(startDay, startDay + numDaysInSideNav));

</script>

{#if !settings.topNav.disable}
	<nav
		style:font-family="'{font}'"
		style:font-size="{getFontInfo(font)?.size || 1}rem"
		style:height={navHeightAdjustments.get(font)
			? `calc(var(--topnav-height) + ${navHeightAdjustments.get(font)})`
			: ''}>
		<ol class="breadcrumbs">
			<li>
				<a href="#{year}" class="home">
					<HomeIcon
						width="1.35rem"
						height="1.35rem"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} />
				</a>
			</li>
			{#if showYearBreadcrumb}
				<li><a href="#{year}">{year}</a></li>
			{/if}
			{#if showQuarterBreadcrumb}
				<li>
					<a href="#{year}-q{quarter}" >
						{!showWeekBreadcrumb && !showMonthBreadcrumb && !showDayBreadcrumb
							? 'Quarter '
							: 'Q'}{quarter}
					</a>
				</li>
			{/if}
			{#if showMonthBreadcrumb}
				<li>
					<a href="#{year}-{month}">
						{new Date(year, month - 1).toLocaleString('default', {
							month: !showWeekBreadcrumb && !showDayBreadcrumb ? 'long' : 'short',
						})}
					</a>
				</li>
			{/if}
			{#if showWeekBreadcrumb}
				<li>
					<a href="#{timeframe.year}-wk{timeframe.weekSinceYear}">
						{#if settings.weekPage.useWeekSinceYear}
							{#if (!showYearBreadcrumb && !showMonthBreadcrumb) || (timeframe.weekYear && timeframe.weekYear !== year) || timeframe.year !== year}
								{timeframe.weekYear || timeframe.year || year}
							{/if}
						{:else if !showMonthBreadcrumb || (timeframe.weekMonth && timeframe.weekYear && timeframe.weekMonth !== timeframe.month) || timeframe.month !== month}
							{new Date(timeframe.weekYear || timeframe.year!, (timeframe.weekMonth || timeframe.month!) - 1).toLocaleString('default', {
								month: !showDayBreadcrumb && (!timeframe.weekMonth || timeframe.weekMonth === timeframe.month) && (!showMonthBreadcrumb || timeframe.month === month)  ? 'long' : 'short',
							})}
						{/if}
						{#if !showDayBreadcrumb}Week{:else}WK{/if}
						{settings.weekPage.useWeekSinceYear
							? timeframe.weekSinceYear
							: timeframe.weekSinceMonth}
					</a>
				</li>
			{/if}
			{#if showDayBreadcrumb}
				<li>
					<a href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}">
						{timeframe.start.toLocaleString('default', {
							weekday: 'short',
							timeZone: 'UTC',
						})},
						{timeframe.start.toLocaleString('default', {
							month: !breadcrumbs.length ? 'long' : 'short',
							timeZone: 'UTC',
						})}
						{@html formatToString(timeframe.daySinceMonth, {
							type: 'ordinal',
							html: true,
						})}
					</a>
				</li>
			{/if}
		</ol>
		<div style="flex: 1" />
<ol class="links">
{#if tabs === 'quarters'}
					{#each settings.quarters as quarter (quarter.id)}
						{#if quarter.year === timeframe.year}
							<li>
								<a
									href="#{quarter.id}">
									{quarter.nameShort}
								</a>
							</li>
						{/if}
					{/each}
				{/if}

{#if showDayBreadcrumb}
     <li><a href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}">Planner</a></li>
      <li><a href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}-pg2">Notes</a></li>
{:else if showWeekBreadcrumb}
     <li><a href="#{timeframe.year}-wk{timeframe.weekSinceYear}">Planner</a></li>
     <li><a href="#{timeframe.year}-wk{timeframe.weekSinceYear}-pg2">Notes</a></li>
{/if}
</ol>
</nav>
{/if}

<style lang="scss">
	:global(main.side-nav-right) nav {
		padding: 0 var(--sidenav-width) 0 0;
	}
	nav {
		display: flex;
		align-items: center;
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: var(--topnav-height);
		padding: 0 0 0 var(--sidenav-width);

		ol.breadcrumbs {
			list-style: none;
			padding: 0;
			margin: 0;
			display: flex;
			height: 100%;
			font-size: 1.2em;
			li {
				display: flex;
				align-items: center;
				height: 100%;
				&:not(:last-child)::after {
					content: '/';
					color: var(--text-low);
					font-size: 0.8em;
					opacity: 0.8;
				}
				&:first-child {
					a {
						padding-left: 1rem;
					}
				}
				&:last-child {
					a {
						color: var(--text-high);
						// font-size: 1.1em;
					}
				}
			}
			a {
				font-size: 1em;
				color: var(--text);
				padding: 0 0.35rem;
				line-height: 1;
				&.home {
					display: flex;
					height: 100%;
					align-items: center;
					color: var(--text-low);
				}
				:global(svg) {
					font-size: 1em;
				}
				:global(.ordinal) {
					color: currentColor;
					font-size: 0.75em;
					vertical-align: top;
				}
			}
		}
		ol.links {
			list-style: none;
			padding: 0;
			margin: 0;
			align-items: right;
			display: flex;
    			padding-top:0px;
			li {
				display: flex;
				text-align: center;
				align-items: center;
				background-color: var(--outline-low);
    				border: none;
    				border-radius: 4px; /* Half the height for perfect rounded corners */
    				color: var(--text-high);
    				text-decoration: none;
    				font-family: Arial, sans-serif;
				width: 70px;
    				height: 25px;
				padding-right: 10px;
				margin-right: 5px;
				font-size:0.65em;
			}
		}
	}
</style>
