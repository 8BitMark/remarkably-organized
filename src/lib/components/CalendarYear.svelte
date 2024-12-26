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
			<a href="#{getMonthLink(month)}" class="month">
				<h2>{month.nameLong}</h2>
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
        {@const weekDate = new Date(month.start.getUTCFullYear(), month.start.getUTCMonth(), day+1)}
					   {@let dayNumber =
								    (weekDate.getUTCDay() === 0 && !startWeekOnSunday) ? weekDate.getUTCDay() + 8 : 
												startWeekOnSunday ? weekDate.getUTCDay() + 2 : 
												weekDate.getUTCDay()+1}

					   	{#if day === 0 || (startWeekOnSunday && dayNumber === 2) || (!startWeekOnSunday &&  === 1)}
           {@const weekNum = getWeek(weekDate, startWeekOnSunday).weekSinceYear}
						     	<a class="day" style:grid-column=1 href="#{month.start.getUTCFullYear()}-wk{weekNum}">{weekDate.getDate()}:{weekNum}</a>
						   {/if}

						<a class="day"
							style:grid-column={dayNumber}
							href="#{month.start.getUTCFullYear()}-{month.start.getUTCMonth() + 1}-{day+1}">
							{weekDate.getDate()}
						</a> 
					{/each}
				</div>
			</a>
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
		justify-items: center;
		align-items: center;
		gap: 0.15rem 0.25rem;
		.label {
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: 0.65em;
			font-weight: var(--font-weight-bold);
			color: var(--text-low);
		}
		.day {
			font-size: 1.1em;
			font-weight: var(--font-weight-light);
			line-height: 1.3rem;
		}
		.week-number {
			font-size: 1.1em;
			font-weight: var(--font-weight-bold);
			line-height: 1.3rem;
			color: var(--text-high);
			background-color: var(--outline);
		}
		.week-label {
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: 0.65em;
			font-weight: var(--font-weight-bold);
			color: var(--text-low);
			background-color: var(--outline);
			border: 1px solid white;
		}
	}
</style>
