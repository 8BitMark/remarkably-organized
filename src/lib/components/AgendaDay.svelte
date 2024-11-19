
<!-- <div class="planner">
	<small>PRIORITIES</small>
	{#each new Array(36) as _, i (i)}
		{#if (((i-1)/2)) % 1 === 0}
			<div class="tasks"> 

			</div>
		{:else}
			<div class="tasks"> 
				blank
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
-->

  <div class="container">
    <div class="left-side">
      <div class="priorities">
        <h2>Priorities</h2>
        <ul>
          {#each Array(6) as _, i}
          <li>
            <input type="checkbox" id={`priority-${i}`} />
          </li>
          {/each}
        </ul>
      </div>
      <div class="notes">
        <h2>Notes</h2>
        <ul>
          {#each Array(20) as _, i}
          <li>&nbsp;</li>
          {/each}
        </ul>
      </div>
    </div>
    <div class="right-side">
      <div class="schedule">
        <h2>Schedule</h2>
        {#each Array(18) as _, i (i)}
        <div class="hour">
          {(i + 6) % 12 === 0 ? 12 : (i + 6) % 12} {i < 6 ? 'AM' : 'PM'}
        </div>
        <div class="half-hour"></div>
        {/each}
      </div>
    </div>



<style lang="scss">
    .container {
      display: grid;
      grid-template-columns: 60% 40%;
      grid-template-rows: repeat(36, 1fr);
      gap: 20px;
      height: 100%;
    }
    .left-side, .right-side {
      display: flex;
      flex-direction: column;
	  padding-left: 10px; 
	}
    .date {
      margin-bottom: 20px;
      font-size: 1.5em;
      font-weight: bold;
    }
    .priorities, .notes, .schedule {
      flex: 1;
    }
    .priorities h2, .notes h2, .schedule h2 {
      margin-top: 0;
      font-size: 1.2em;
      color: #555;
      padding: 2px;
    }
    .priorities ul, .notes ul {
      list-style-type: none;
      padding: 0;
    }
    .priorities li, .notes li {
      border-bottom: 1px solid #ccc;
    }
    .notes ul {
      height: calc(100% - 70px); /* Adjust for the header height */
    }
    .notes li::after {
      display: block;
      border-bottom: 1px solid #ccc;
    }
    .schedule {
      display: grid;
      grid-template-rows: repeat(34, 1fr);
      gap: 5px;
    }
    .hour{
      border-bottom: 1px solid darkgrey;
      padding: 2px;
    }
	.half-hour {
	  border-bottom: 1px dashed lightgrey;
	}



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
		hour {
			border-top: solid 2px;
			color: darkgrey;
			width:100%;
			small {
				font-size: 1em;
			}
		}
		halfhour {
			border-top: solid 1px;
			width:100%;
			color: lightgray;
		}
	 }
	}
</style>
