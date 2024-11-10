

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
</div>

<style lang="scss">

		.tasks {
		height: 100%;
		width: 100%;
		justify-items: stretch;
		align-items: left;
		grid-auto-flow: column;
		padding: 1rem 1rem 0 0;
	}

	.schedule {
		display: grid; 
		grid-template-columns: 60% 40%; 
		grid-template-rows: repeat(18, 1fr);
		height: 100%;
		width: 100%;
		justify-items: stretch;
		align-items: right;
		grid-auto-flow: column;
		padding: 1rem 1rem 0 0;
	}

	.hour {
		border-top: solid 1px var(--outline);
	}
	.hour-label {
		text-align: left;
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
