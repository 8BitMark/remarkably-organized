<div class="planner">
	<small>PRIORITIES</small>
	{#each new Array(36) as _, i (i)}
		<div class="tasks"> 
		</div>
	{/each}




	{#each new Array(36) as _, i (i)}
        <div class="schedule">
            {#if (((i-1)/2) + 6) % 1 === 0}
                {(((i-1)/2) + 6) === 12 ? 12 : (((i-1)/2) + 6) % 12}
                <small>{(((i-1)/2) + 6) < 12 ? 'AM' : 'PM'}</small>
			{/if}
            
            {#if (((i-1)/2) + 6) % 1 === 0.5}
                <small>...</small>
            {/if}
        </div>
	{/each}

</div>

<style lang="scss">
	.planner {
		display: grid; 
		grid-template-columns: 60% 40%; 
		grid-template-rows: repeat(36, 1fr);
		height: 100%;
		width: 100%;
		justify-items: stretch;
		align-items: right;
		grid-auto-flow: column;
		padding: 1rem 1rem 0 0;
	

	.tasks {
		border-top: solid 2px var(--outline);
		text-align: left;
		grid-column: 1;
		font-weight: var(--font-weight-light);
		font-size: 0.7em;
		color: var(--text-low);
		margin-top: -0.5rem;
		padding: 1rem 1rem 0 1rem;
	}

	.schedule {
		border-top: solid 1px var(--outline);
		text-align: left;
		grid-column: 2;
		font-weight: var(--font-weight-light);
		font-size: 0.7em;
		color: var(--text-low);
		margin-top: -0.5rem;
		small {
			color: currentColor;
			font-size: 1em;
		}
	}
	}
</style>
