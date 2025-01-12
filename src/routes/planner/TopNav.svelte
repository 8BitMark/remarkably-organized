<script lang="ts">
	import { formatToString, PlannerSettings, type Timeframe } from '$lib';
	import HomeIcon from '~icons/fluent/calendar-32-filled';
	import QuarterIcon from '~icons/fluent/calendar-empty-32-regular';
	import MonthIcon from '~icons/fluent/calendar-ltr-32-regular';
	import PlannerIcon from '~icons/fluent/text-bullet-list-square-32-regular';
	import NotepadIcon from '~icons/fluent/clipboard-text-edit-32-regular';
	import WeekIcon from '~icons/fluent/calendar-empty-32-regular';
	
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
	<nav style:font-family="'{font}'">
	<div class="left-header">
		<a href="#{year}">
			<HomeIcon
				width="32px"
				height="32px"/>
		</a>
		{#if showYearBreadcrumb && tabs === 'years'}
			<a class="title" href="#{year}">{year}</a>
		{:else if showQuarterBreadcrumb && tabs === 'quarters'}
			<a class="title" href="#{year}-q{quarter}">Quarter {quarter}</a>
		{:else if showMonthBreadcrumb && tabs === 'months'}
			<a class="title" href="#{year}-{month}">{new Date(year, month - 1).toLocaleString('default', { month: 'long' })}</a>
		{:else if showWeekBreadcrumb && (tabs === 'weeks-this-month' || tabs === 'weeks-this-year')}
			<a class="title" href="#{timeframe.year}-wk{timeframe.weekSinceYear}">{new Date(year, month - 1).toLocaleString('default', { month: 'long' })} - Week {settings.weekPage.useWeekSinceYear
				? timeframe.weekSinceYear
				: timeframe.weekSinceMonth}</a>
		{:else if showDayBreadcrumb && (tabs === 'days-this-week' || tabs === 'days-this-month' || tabs === 'days-this-year')}
			<a class="title" href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}">
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
	</div>		
					
	<div class="right-header">
		{#if showQuarterBreadcrumb && tabs !== 'quarters'}
				<a class="icon" href="#{year}-q{quarter}" >
					<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
						<path fill="currentColor" d="M2 6.5A4.5 4.5 0 0 1 6.5 2h15A4.5 4.5 0 0 1 26 6.5v15a4.5 4.5 0 0 1-4.5 4.5h-15A4.5 4.5 0 0 1 2 21.5zM6.5 4A2.5 2.5 0 0 0 4 6.5V7h20v-.5A2.5 2.5 0 0 0 21.5 4zM4 21.5A2.5 2.5 0 0 0 6.5 24h15a2.5 2.5 0 0 0 2.5-2.5V9H4zm24-12V5.757c1.206.808 2 2.183 2 3.743V22a8 8 0 0 1-8 8H9.5a4.5 4.5 0 0 1-3.742-2H22a6 6 0 0 0 6-6z"/>
                                                <text x="10" y="21" text-anchor="middle" fill="black" font-size="12">{quarter}</text>
					</svg>
				</a>
		{/if}
		{#if showMonthBreadcrumb && (tabs === 'days-this-week' || tabs === 'days-this-month' || tabs === 'days-this-year' || tabs === 'weeks-this-month' || tabs === 'weeks-this-year')}	
				<a class="icon" href="#{year}-{month}">
					<svg xmlns="http://www.w3.org/2000/svg" width="34" height="34" viewBox="0 0 34 34">
  <path fill="currentColor" d="m4.25 9.563a5.313 5.313 0 0 1 5.313-5.312h14.875a5.313 5.313 0 0 1 5.313 5.313v14.875a5.313 5.313 0 0 1-5.312 5.313h-14.875a5.313 5.313 0 0 1-5.312-5.312v-4.25a1.063 1.063 0 1 1 2.125 0v4.25a3.188 3.188 0 0 0 3.188 3.188h14.875a3.188 3.188 0 0 0 3.188-3.187v-11.687h-16.692a2.656 2.656 0 0 0 0-2.125h16.692v-1.062a3.188 3.188 0 0 0-3.187-3.187h-14.875a3.188 3.188 0 0 0-3.187 3.188v2.748l1.374-1.374a1.063 1.063 0 1 1 1.502 1.502l-3.187 3.188a1.063 1.063 0 0 1-1.502 0l-3.188-3.187a1.063 1.063 0 1 1 1.502-1.502l1.371 1.371"/>
  					<text x="16" y="24" text-anchor="middle" fill="black" font-size="14">
							{timeframe.start.toLocaleString('default', { month: 'short' })}</text>
					</svg>
				</a>
		{/if}
		{#if showWeekBreadcrumb && (tabs === 'days-this-week' || tabs === 'days-this-month' || tabs === 'days-this-year' )}
				<a class="icon" href="#{timeframe.year}-wk{timeframe.weekSinceYear}">
					<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
					<path fill="currentColor" d="M7.5 3A4.5 4.5 0 0 0 3 7.5v17A4.5 4.5 0 0 0 7.5 29h17a4.5 4.5 0 0 0 4.5-4.5v-17A4.5 4.5 0 0 0 24.5 3zM5 7.5A2.5 2.5 0 0 1 7.5 5h17A2.5 2.5 0 0 1 27 7.5V9H5zM5 11h22v13.5a2.5 2.5 0 0 1-2.5 2.5h-17A2.5 2.5 0 0 1 5 24.5z"/>
					<text x="16" y="24" text-anchor="middle" fill="black" font-size="14">
							{settings.weekPage.useWeekSinceYear
								? timeframe.weekSinceYear
								: timeframe.weekSinceMonth}</text>
					</svg>
				</a>	
		{/if}

     <ol class="links">
		{#if tabs === 'quarters'}
			{#each settings.quarters as quarters (quarters.id)}
			{@const isActive = quarter === parseInt(quarters.nameShort.charAt(1),10)}
				<a href="#{quarters.id}" class:active={isActive}><li>
					{quarters.nameShort}</li></a>
			{/each}
		{/if}
		</ol>
		{#if  (tabs === 'weeks-this-year' || tabs === 'weeks-this-month')}
			{#each weeks as week, i (week.id)}
				{@const isActive =
					!disableActiveIndicator && timeframe.weekSinceYear === week.weekSinceYear}
					<a href="#{week.id}"
						class:active={isActive} style="text-transform: lowercase"><small>w</small>{settings.weekPage.useWeekSinceYear
								? week.weekSinceYear
								: week.weekSinceMonth}
						</a>
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
						class:highlight-end={highlighEnd}>
							{day.start.toLocaleString('default', {
								weekday: 'short',
								timeZone: 'UTC',
							}).charAt(0)}
					</a>
			{/each}
		{/if}

		{#if tabs === 'months'}
			{#each settings.months as month (month.id)}
				{#if month.year === timeframe.year}
					<a 
						href="#{month.id}"
						class:active={!disableActiveIndicator &&
							timeframe.month === month.month}>
						{month.nameShort.charAt(0)}
					</a>
				{/if}
			{/each}
		{/if}

	{#if showDayBreadcrumb}
	     {@const isActive = breadcrumbs?.length > 0}
	     <a class="icon" href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}">
							<PlannerIcon
							width="32px"
							height="32px"/>
							</a>
	     <a class="icon" href="#{timeframe.year}-{timeframe.month}-{timeframe.daySinceMonth}-pg2">
							<NotepadIcon
							width="32px"
							height="32px"/>
							</a>
	{:else if showWeekBreadcrumb}
	     {@const isActive = breadcrumbs?.length > 0}
	     <a class="icon" href="#{timeframe.year}-wk{timeframe.weekSinceYear}">
							<PlannerIcon
							width="32px"
							height="32px"/>
							</a>
	     <a class="icon" href="#{timeframe.year}-wk{timeframe.weekSinceYear}-pg2">
							<NotepadIcon
							width="32px"
							height="32px"/>
							</a>
	{:else if showMonthBreadcrumb}
	     {@const isActive = breadcrumbs?.length > 0}
	     <a class="icon" href="#{timeframe.year}-{timeframe.month}">
							<PlannerIcon
							width="32px"
							height="32px"/>
							</a>
	     <a class="icon" href="#{timeframe.year}-{timeframe.month}-pg2">
							<NotepadIcon
							width="32px"
							height="32px"/>
							</a>
	{:else if showQuarterBreadcrumb}
	     {@const isActive = breadcrumbs?.length > 0}
	     <a class="icon" href="#{year}-q{quarter}">
							<PlannerIcon
							width="32px"
							height="32px"/>
							</a>
	     <a class="icon" href="#{year}-q{quarter}-pg2">
							<NotepadIcon
							width="32px"
							height="32px"/>
							</a>
	{/if}
	</div>
</nav>
{/if}

<style lang="scss">
	:global(main.side-nav-right) nav {
		padding: 0 var(--sidenav-width) 0 0;
	}
	nav {
		display: grid;
      		grid-template-columns: auto auto; /* Two columns with auto widths */
     		grid-template-rows: 1fr;
      		gap: 0px;
		align-items: start; /* Align items to the top */
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: var(--topnav-height);
		padding: 0 0 0 var(--sidenav-width);
		margin-top: 5px;
    		margin-left: 10px;
		font-size: 1.2em;
	}
	.left-header {
		grid-column:1;
                display:flex;
		font-size: 1.5em;
		align-items: start;
		justify-content: flex-start;
	}
	.right-header {
		grid-column:2;
                display:flex;
		text-align: right;
    		margin-right: 5px;
                justify-content: flex-end;
                align-items:center;
	}
	.title {
		vertical-align: text-top;
	}
	buttons {
         a {
		display: inline-block;
		text-align: center;
		vertical-align: top;
		background-color: var(--nav-bg);
		border: 1.8px solid black;
		border-radius: 4px; /* Half the height for perfect rounded corners */
		color: var(--text-high);
		text-decoration: none;
		height: 26px;
		width: 26px;
		margin-right: 3px;
                margin-left:3px;
		font-size:0.95em;
		text-transform: uppercase;
		padding-top:3px;
	}
	a.active {
		background-color: var(--fg-text-low);
	}
}

	.icon {
		text-decoration: none;
		color: var(--text);
    		margin-top: 4px;
	}

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
			margin-right: 0px;
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
</style>
