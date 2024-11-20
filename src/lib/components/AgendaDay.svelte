
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
        {#each Array(36) as _, i (i)}
        <div class="hour">
          {(i + 6) % 12 === 0 ? 12 : (i + 6) % 12} {i < 6 ? 'AM' : 'PM'}
        </div>
        <div class="half-hour"></div>
        {/each}
      </div>
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
      border-top: 1px solid darkgrey;
      padding: 2px;
    }
    .half-hour {
      border-top: 1px dashed lightgrey;
    }
</style>
