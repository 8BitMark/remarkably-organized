<script lang="ts">
	import { formatToString, getFirstDayOfWeek, type Timeframe } from '$lib';

	let { timeframe = {} as Timeframe, startWeekOnSunday = false } = $props();

	const weekStart = $derived(
		new Date(getFirstDayOfWeek(timeframe.start, startWeekOnSunday)),
	);
</script>

<div class="container">
    <div class="left-side">

    	<div class="weekly-plan">
    	<h2>Weekly Plan</h2>
		{#each new Array(7) as _, i (i)}
		{@const date = new Date(weekStart.getTime() + i * 86400000)}
		{#if timeframe.weekStart}
			<a class="day"
				href="#{date.getUTCFullYear()}-{date.getUTCMonth() + 1}-{date.getUTCDate()}">
				{date.toLocaleString('default', { weekday: 'short', timeZone: 'UTC' })} {date.getUTCDate()}
			</a>
		{:else}
			<div class="day">
				{date.toLocaleString('default', { weekday: 'short', timeZone: 'UTC' })}
			</div>
		{/if}
  <div class="content"></div>
	    	{/each}
	</div>
    </div>

    <div class="right-side">
      <div class="priorities">
	<h2>Weekly Targets</h2>
        {#each Array(8) as _, i}
	    <div class="icon"></div>
	    <div class="content"></div>
        {/each}
      </div>

      <div class="priorities">
        <h2>Tasks</h2>
        {#each Array(8) as _, i}
	    <div class="icon"></div>
	    <div class="content"></div>
        {/each}
      </div>

      <div class="notes">
        <h2>Notes</h2>
        {#each Array(16) as _, i}
          <div class="lines"></div>
        {/each}
      </div>
    </div>
</div>

<style lang="scss">
    .container {
      display: grid;
      grid-template-columns: 40% 60%;
      grid-template-rows: repeat(17, 1fr);
      gap: 5px;
      height: 95%;
      widgth: 95%;
    }

    .left-side, .right-side {
      display: flex;
      flex-direction: column;
      padding-left: 10px;  
    }

   .right-side {
	   padding-right: 10px;  
   }

   .priorities {
    display: grid;
    grid-template-columns: 24px auto;
    gap: 0px;
  }
  .priorities h2 {
     grid-column:1 / span 2;
     border-bottom: 1.5px solid var(--outline-high);
   }
  .priorities .icon {
    grid-column:1;
    width: 25px;
    height: 24px;
    border-right: 1px solid var(--outline-high);
    border-bottom: 1px solid var(--outline-high);
  }

  .priorities .content {
     grid-column:2;
     padding: 0px;
     border-bottom: 1px solid var(--outline-high);
     font-size: 0.9em;
     height: 24px;
     width: auto;
  }

  .weekly-plan {
    display: grid;
    grid-template-columns: 24px auto;
    grid-template-rows: 25px repeat(7, 1fr);
    gap: 0px;
    height: 98%;
				margin: 0px;
  }

  .weekly-plan .day {
    grid-column:1;
    width: 25px;
				height: auto;
				border-left: 1px solid var(--outline-high);
    border-bottom: 1px solid var(--outline-high);
    background-color: var(--outline);
    padding-top: 3px;
  }

.weekly-plan .date {
	display:flex;
	color: var(--text-low);
    writing-mode: vertical-lr;
    text-orientation: mixed;
		  margin: 0px;
    /* transform: rotate(180deg); /* Rotate text to start from bottom */
    align-items: center; /* Align text to the bottom of the cell */
    justify-content: center; /* Center text horizontally */
    letter-spacing: 1.5px;
    text-transform: uppercase;
	}

  .weekly-plan .content {
     grid-column:2;
     padding: 0px;
					margin: 0px;
					border-left: 1px solid var(--outline-high);
     border-bottom: 1px solid var(--outline-high);
     font-size: 0.9em;
     width: auto;
					height:auto;
  }

  .notes, .schedule {
    flex: 1;
    display: grid;
    gap: 2px;
  }

  .priorities h2, .notes h2, .schedule h2 {
    border-bottom: 2px solid var(--outline-high);
    height: 25px;
    margin-top: 5px;
    font-size: 1.2em;
    color: var(--text-low);
    padding-top: 5px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
  }

  .weekly-plan h2 {
		   grid-column:1 / span 2;
     border-bottom: 2px solid var(--outline-high);
					height: 25px;
     margin-top: 5px;
					margin-bottom: 5px;
     font-size: 1.2em;
     color: var(--text-low);
     padding-top: 5px;
     letter-spacing: 1.5px;
     text-transform: uppercase;
   }

.schedule {
	padding-right: 10px;
}
  
    .hour, .lines{
      border-bottom: 1px solid var(--outline-high);
      font-size: 0.7em;
      height: 22px;
      padding-top: 3px;
    }
    .half-hour {
      height: 22px;
      border-bottom: 1px dashed var(--outline);
    }
</style>
