<div class="planner">
	<small>PRIORITIES</small>
	{#each new Array(36) as _, i (i)}
		{#if (((i-1)/2)) % 1 === 0}
			<div class="tasks"> 
			</div>
		{/if}
	{/each}




	{#each new Array(36) as _, i (i)}
        <div class="schedule">
            {#if (((i-1)/2) + 6) % 1 === 0}
                <hour>
					{(((i-1)/2) + 6) === 12 ? 12 : (((i-1)/2) + 6) % 12}
					<small>{(((i-1)/2) + 6) < 12 ? 'AM' : 'PM'}</small>
				</hour>
			{/if}
            
            {#if (((i-1)/2) + 6) % 1 === 0.5}
                <halfhour>

				</halfhour>
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
		text-align: left;
		grid-column: 1;
		font-weight: var(--font-weight-light);
		font-size: 0.7em;
		color: var(--text-low);
		margin-top: -0.5rem;
		margin-left: 10px;
		margin-right: 10px;
		padding: 1rem 1rem 0 1rem;
		border-top: solid 2px var(--outline);
		color: darkgrey;
		font-size: 1em;
	}

	.schedule {
		text-align: left;
		grid-column: 2;
		font-weight: var(--font-weight-light);
		font-size: 0.7em;
		color: var(--text-low);
		margin-top: -0.5rem;
		margin-left: 10px;
		margin-right: 10px;
		width:100%;
		hour {
			border-top: solid 2px var(--outline);
			color: darkgrey;
			small {
				font-size: 1em;
			}
		}
		halfhour {
			border-top: solid 1px var(--outline);
			color: lightgray;
		}
	 }
	}
</style>
