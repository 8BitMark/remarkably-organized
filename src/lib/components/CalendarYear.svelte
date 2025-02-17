<script lang="ts">
	import type { Month, PlannerSettings} from '$lib';
 import {getWeek} from '$lib';
	import { formatToString } from '$lib';

	let {
		settings = {} as PlannerSettings,
		months = [] as Month[],
		startWeekOnSunday = false,
	} = $props();

	function getMonthLink(month: Month) {
		if (!settings.monthPage) return month.id;
		if (!settings.monthPage.disable) return month.id;
		if (!settings.weekPage.disable) {
			const week = settings.weeks.find(
				(week) => week.month === month.month && week.year === month.year,
			);
			return week ? week.id : '';
		}
		if (!settings.dayPage.disable) {
			const day = settings.days.find(
				(day) => day.year === month.year && day.month === month.month,
			);
			return day ? day.id : '';
		}
		return month.id;
	}

	function capitalizeFirstLetter(string: string) {
		if (!string) return '';
		return string.charAt(0).toUpperCase() + string.slice(1);
	}

	function getDayShortName(offset = 0) {
		const day = capitalizeFirstLetter(
			formatToString(
				new Date().setDate(new Date().getDate() - new Date().getDay() + offset),
				{ type: 'date', weekday: 'short' },
			),
		);
		if (day === 'Mon') return 'Mo';
		if (day === 'Tue') return 'Tu';
		if (day === 'Wed') return 'We';
		if (day === 'Thu') return 'Th';
		if (day === 'Fri') return 'Fr';
		if (day === 'Sat') return 'Sa';
		if (day === 'Sun') return 'Su';
		return day;
	}
</script>

{#if months.length}
	<div class="months">
		{#each months as month (month.id)}
			<div class="month">
			<a href="#{getMonthLink(month)}">
				<h2>{month.nameLong}</h2></a>
				<div class="days">
					<div class="week-label">Wk</div>
					{#if startWeekOnSunday}
						<div class="label">{getDayShortName(0)}</div>
					{/if}
					<div class="label">{getDayShortName(1)}</div>
					<div class="label">{getDayShortName(2)}</div>
					<div class="label">{getDayShortName(3)}</div>
					<div class="label">{getDayShortName(4)}</div>
					<div class="label">{getDayShortName(5)}</div>
					<div class="label">{getDayShortName(6)}</div>
					{#if !startWeekOnSunday}
						<div class="label">{getDayShortName(0)}</div>
					{/if}
					{#each new Array(month.end.getUTCDate()) as _, day}
	       {@const weekDate = new Date(`${month.start.getUTCFullYear()}-${String(month.start.getUTCMonth() + 1).padStart(2, '0')}-${String(day + 1).padStart(2, '0')}`)}
						  {@const dayNumber =
								    (weekDate.getUTCDay() === 0 && !startWeekOnSunday) ? weekDate.getUTCDay() + 7 : 
												startWeekOnSunday ? weekDate.getUTCDay() + 1 : 
												weekDate.getUTCDay()}

					   	{#if day === 0 || dayNumber === 1}
           {@const weekNum = getWeek(weekDate, startWeekOnSunday).weekSinceYear}
						     	<a class="week-number" style:grid-column=1 href="#{month.start.getUTCFullYear()}-wk{weekNum}">{weekNum}</a>
						   {/if}

						<a class="day"
							style:grid-column={dayNumber+1}
							href="#{month.start.getUTCFullYear()}-{month.start.getUTCMonth() + 1}-{day+1}">
							{weekDate.getDate()}
						</a> 
					{/each}
				</div>
			</div>
		{/each}
	</div>
{/if}

<style lang="scss">
	.months {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-template-rows: repeat(4, 1fr);
		align-items: start;
		gap: 0rem 1.5rem;
		flex: 1;
		width: 100%;
		height: 100%;
		padding: 0 1.5rem 3.5rem;
		h2 { 
			text-align: center;
			font-size: 1.2em;
			font-weight: var(--font-weight-normal);
			padding: 0 0 0.25rem;
			line-height: 1.2rem;
		}
	}
	.days {
		display: grid;
		grid-template-columns: repeat(8, 1fr);
		grid-template-rows: repeat(7, 1fr);
		justify-items: right;
		align-items: right;
		margin: 0.1rem 0.25rem;
		.label {
			display: flex;
			align-items: right;
			justify-content: right;
			font-size: 0.85em;
			font-weight: var(--font-weight-bold);
			color: var(--text-high);
			border-bottom: 1px solid var(--outline-low);
			width:100%;
		}
		
		
		.day {
			font-size: 1em;
			font-weight: var(--font-weight-light);
			line-height: 1.3rem;
		}
		.week-number {
			font-size: 1em;
			font-weight: var(--font-weight-bold);
			line-height: 1.3rem;
			color: var(--text-low);
			border-right: 1px solid var(--outline-low);
			align-items: right;
			justify-content: right;
			text-align: right;
			padding-right: 5px;
		}
		.week-label {
			display: flex;
			align-items: right;
			justify-content: right;
			font-size: 0.85em;
			font-weight: var(--font-weight-bold);
			color: var(--text-low);
			border-bottom: 1px solid var(--outline-low);
			padding-right: 5px;
		}
	}
</style>
