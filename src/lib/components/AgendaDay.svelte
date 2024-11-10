
		<div class="tasks">
		       <medium>Priorities</medium>
		</div>

		<div class="schedule">
			{#each new Array(17) as _, i (i)}
				<div class="hour-label">
					{#if i > 0}
						{(i+6) === 12 ? 12 : (i+6) % 12}
						<small>{(i+6) < 12 ? 'AM' : 'PM'}</small>
					{:else}
						6
						<small>AM</small>
					{/if}
		</div>

	{/each}
	{#each new Array(17) as _, i (i)}
		<div class="hour"></div>
	{/each}
</div>

<style lang="scss">
	.tasks {
		display: grid;
		grid-template-columns: 1.5rem 1fr;
		grid-template-rows: repeat(18, 1fr);
		height: 50%;
		width: 60%;
		justify-items: stretch;
		align-items: left;
		grid-auto-flow: column;
		padding: 1rem 1rem 0 0;
	}

	.schedule {
		display: grid;
		grid-template-columns: 1rem 1fr;
		grid-template-rows: repeat(18, 1fr);
		height: 50%;
		width: 40%;
		justify-items: stretch;
		align-items: right;
		grid-auto-flow: column;
		padding: 1rem 1rem 0 0;
	}

	.hour {
		border-top: solid 1px var(--outline);
	}
	.hour-label {
		text-align: center;
		grid-column: 1;
		font-weight: var(--font-weight-light);
		font-size: 0.7em;
		color: var(--text-low);
		margin-top: -0.5rem;
		small {
			color: currentColor;
			font-size: 0.6em;
		}
	}
</style>
