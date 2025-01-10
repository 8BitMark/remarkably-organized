<script lang="ts">
	import { formatToString, PlannerSettings, type Timeframe } from '$lib';
	import HomeIcon from '~icons/fluent/calendar-28-filled';
	import QuarterIcon from '~icons/fluent/calendar-reply-28-regular';
	import MonthIcon from '~icons/fluent/calendar-arrow-counterclockwise-28-regular';
	import PlannerIcon from '~icons/fluent/calendar-clock-24-regular';
	import NotepadIcon from '~icons/fluent/notepad-28-regular';
	import WeekIcon from '~icons/fluent/calendar-empty-28-regular';
	
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
		<a href="#{year}" class="home">
			<HomeIcon
				width="28px"
				height="28px"
				style={homeIconAdjustments.get(font)
					? `margin-top: ${homeIconAdjustments.get(font)}`
					: null} />
		</a>
		{#if showYearBreadcrumb && tabs === 'years'}
			<a href="#{year}">{year}</a>
		{:else if showQuarterBreadcrumb && tabs === 'quarters'}
			<a href="#{year}-q{quarter}">Quarter {quarter}</a>
		{:else if showMonthBreadcrumb && tabs === 'months'}
			<a href="#{year}-{month}">{new Date(year, month - 1).toLocaleString('default', { month: 'long' })}</a>
		{:else if showWeekBreadcrumb && (tabs === 'weeks-this-month' || tabs === 'weeks-this-year')}
			<a href="#{timeframe.year}-wk{timeframe.weekSinceYear}">Week {settings.weekPage.useWeekSinceYear
				? timeframe.weekSinceYear
				: timeframe.weekSinceMonth}</a>
		{:else if showDayBreadcrumb && (tabs === 'days-this-week' || tabs === 'days-this-month' || tabs === 'days-this-year')}
			<a href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}">
					{timeframe.start.toLocaleString('default', {
						weekday: 'long',
						timeZone: 'UTC',
					})},
					{@html formatToString(timeframe.daySinceMonth, {
						type: 'd',
						html: true,
					})}
					{timeframe.start.toLocaleString('default', {
						month: 'long',
						timeZone: 'UTC',
					})}</a>
		{/if}
			
					

		<ol class="breadcrumbs">
			{#if showQuarterBreadcrumb && tabs !== 'quarters'}
				<li>
					<a href="#{year}-q{quarter}" >
						<QuarterIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} />
					</a>
				</li>
			{/if}
			{#if showMonthBreadcrumb && (tabs === 'days-this-week' || tabs === 'days-this-month' || tabs === 'days-this-year' || tabs === 'weeks-this-month' || tabs === 'weeks-this-year')}
				<li>
					<a href="#{year}-{month}">
						<svg xmlns="http://www.w3.org/2000/svg" width="32" height="28" viewBox="0 0 28 28" style={homeIconAdjustments.get(font)
												? `margin-top: ${homeIconAdjustments.get(font)}`
												: n}>
								<path fill="currentColor" d="M21.75 3A3.25 3.25 0 0 1 25 6.25v15.5A3.25 3.25 0 0 1 21.75 25H6.25A3.25 3.25 0 0 1 3 21.75V6.25A3.25 3.25 0 0 1 6.25 3zm1.75 6.503h-19V21.75c0 .966.784 1.75 1.75 1.75h15.5a1.75 1.75 0 0 0 1.75-1.75zM21.75 4.5H6.25A1.75 1.75 0 0 0 4.5 6.25v1.753h19V6.25a1.75 1.75 0 0 0-1.75-1.75" />
								<text x="14" y="21" text-anchor="middle" fill="black" font-size="12">
											{timeframe.start.toLocaleString('default', { month: 'short' })}</text>
						</svg>
					</a>
				</li>
			{/if}
			{#if showWeekBreadcrumb && (tabs !== 'weeks-this-year' || tabs !== 'weeks-this-month' || tabs !== 'months' || tabs !== 'quarters')}
				<li>
					<a href="#{timeframe.year}-wk{timeframe.weekSinceYear}">
	<svg xmlns="http://www.w3.org/2000/svg" width="32" height="28" viewBox="0 0 28 28" style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: n}>
			<path fill="currentColor" d="M21.75 3A3.25 3.25 0 0 1 25 6.25v15.5A3.25 3.25 0 0 1 21.75 25H6.25A3.25 3.25 0 0 1 3 21.75V6.25A3.25 3.25 0 0 1 6.25 3zm1.75 6.503h-19V21.75c0 .966.784 1.75 1.75 1.75h15.5a1.75 1.75 0 0 0 1.75-1.75zM21.75 4.5H6.25A1.75 1.75 0 0 0 4.5 6.25v1.753h19V6.25a1.75 1.75 0 0 0-1.75-1.75" />
<text x="14" y="21" text-anchor="middle" fill="black" font-size="12">
						{settings.weekPage.useWeekSinceYear
							? timeframe.weekSinceYear
							: timeframe.weekSinceMonth}</text>
	</svg>
					</a>
				</li>
				
			{/if}
		</ol>

<ol class="links">
{#if tabs === 'quarters'}
	{#each settings.quarters as quarters (quarters.id)}
		{@const isActive = quarter === parseInt(quarters.nameShort.charAt(1),10)}
		<a href="#{quarters.id}" class:active={isActive}>
			<li>{quarters.nameShort}</li></a>
	{/each}
{/if}

{#if tabs === 'days-this-year' || tabs === 'days-this-month' || tabs === 'days-this-week'}
	{#each days as day, i (day.id)}
		{@const isActive =
			!disableActiveIndicator && timeframe.daySinceYear === day.daySinceYear}
		{@const isSaturday = day.start.getUTCDay() === 6}
		{@const isSunday = day.start.getUTCDay() === 0}
		{@const isWeekend = isSaturday || isSunday}
		{@const shouldHighlight = !isActive && isWeekend && tabs !== 'days-this-week'}
		{@const highlightStart = shouldHighlight && isSaturday && i < days.length - 1}
		{@const highlighEnd = shouldHighlight && isSunday && i > 0}
			<a
				href="#{day.id}"
				class:active={isActive}
				class:highlight={shouldHighlight}
				class:highlight-start={highlightStart}
				class:highlight-end={highlighEnd}><li>
				
					{day.start.toLocaleString('default', {
						weekday: 'short',
						timeZone: 'UTC',
					}).charAt(0)}
				
			</li></a>
	{/each}
{/if}

</ol>

{#if showDayBreadcrumb}
     {@const isActive = breadcrumbs?.length > 0}
     <a href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}">
						<PlannerIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} /></a>
     <a href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}-pg2">
						<NotepadIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} /></a>
{:else if showWeekBreadcrumb}
     {@const isActive = breadcrumbs?.length > 0}
     <a href="#{timeframe.year}-wk{timeframe.weekSinceYear}">
						<PlannerIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} /></a>
     <a href="#{timeframe.year}-wk{timeframe.weekSinceYear}-pg2">
						<NotepadIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} /></a>
{:else if showMonthBreadcrumb}
     {@const isActive = breadcrumbs?.length > 0}
     <a href="#{timeframe.year}-{timeframe.month}">
						<PlannerIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} /></a>
     <a href="#{timeframe.year}-{timeframe.month}-pg2">
						<NotepadIcon
						width="28px"
						height="28px"
						style={homeIconAdjustments.get(font)
							? `margin-top: ${homeIconAdjustments.get(font)}`
							: null} /></a>
{/if}

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
		margin-top: 5px;
    		margin-left: 10px;
		font-size: 1.3em;

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
			margin-right: 10px;
			align-items: right;
			display: flex;
    			padding: 0px;
			a {
				display: inline-block;
				text-align: center;
				align-items: center;
				vertical-align: middle;
				background-color: var(--nav-bg);
    				border: 1.5px solid black;
    				border-radius: 4px; /* Half the height for perfect rounded corners */
    				color: var(--text-high);
    				text-decoration: none;
    				height: 25px;
				width: 25px;
				margin-right: 5px;
				font-size:0.95em;
				text-transform: uppercase;
				padding-top:3px;
			}
			a.active {
				background-color: var(--fg-text-low);
			}
		}			
	}
</style>
