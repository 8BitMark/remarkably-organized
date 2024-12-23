<script lang="ts">
	import type { Month, PlannerSettings} from '$lib';
	import { getWeek } from '$lib';
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


	function getWeekNumber(date) {
   		const targetDate = new Date(date);
    		const startOfYear = new Date(targetDate.getFullYear(), 0, 1);
    		const pastDaysOfYear = (targetDate - startOfYear) / 86400000;

    		return Math.ceil((pastDaysOfYear + startOfYear.getDay() + 1) / 7);
  	}


	function getISOWeekNumber(date, startOnSunday = false) {
		const targetDate = new Date(date);
		const dayOfWeek = targetDate.getUTCDay();
		const dayOfYear = Math.floor((targetDate - new Date(targetDate.getUTCFullYear(), 0, 1)) / 86400000) + 1;

		// Adjust dayOfWeek for ISO week (Sunday = 0, Monday = 1)
		const adjustedDayOfWeek = startOnSunday ? dayOfWeek : (dayOfWeek === 0 ? 7 : dayOfWeek);

		// Calculate the week number
		const weekNumber = Math.ceil((dayOfYear + 6 - adjustedDayOfWeek) / 7);

		// Handle the case where the week number is 0 (belongs to the last week of the previous year)
		if (weekNumber === 0) {
			const lastDayOfLastYear = new Date(targetDate.getUTCFullYear() - 1, 11, 31);
			return getISOWeekNumber(lastDayOfLastYear, startOnSunday);
		}

		return weekNumber;
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
					{#each new Array(month.end.getUTCDate()) as caldate, day}

						<!-- <a href="#{week.id}" class="week" class:last-week={i === numWeeks - 1}> -->
						
						{#if day === 0 : startWeekOnSunday ? (month.start.getUTCDay() + day) % 7 === 0 : (month.start.getUTCDay() + day) % 7 === 1}
       {@const weekDate = new Date(month.start.getUTCFullYear(), month.start.getUTCMonth(), day+1)}
							<a class="day" style:grid-column=1 href="5">{getISOWeekNumber(weekDate, startWeekOnSunday)}</a>
						
						{/if}
						<a class="day"
							style:grid-column={day > 0
								? null
								: ((month.start.getUTCDay() - (startWeekOnSunday ? 0 : 1) + 7) % 7) + 2}
							href="#{month.start.getUTCFullYear()}-{month.start.getUTCMonth() + 1}-{day+1}">
							{day+1}
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
		grid-template-rows: repeat(6, 1fr);
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
			color: var(--outline);
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
