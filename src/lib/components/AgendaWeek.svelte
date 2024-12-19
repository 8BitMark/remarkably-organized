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
			<a
				class="day"
				href="#{date.getUTCFullYear()}-{date.getUTCMonth() + 1}-{date.getUTCDate()}">
				{date.toLocaleString('default', { weekday: 'short', timeZone: 'UTC' })}
				{@html formatToString(date.getUTCDate(), { type: 'ordinal', html: true })}
			</a>
			<div class="content"></div>
		{:else}
			<div class="day">
				{date.toLocaleString('default', { weekday: 'short', timeZone: 'UTC' })}
			</div>
			<div class="content"></div>
		{/if}
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
      grid-template-columns: 45% 55%;
      grid-template-rows: repeat(17, 1fr);
      gap: 5px;
      height: 98%;
      widgth: 95%;
    }
    .left-side, .right-side {
      display: flex;
      flex-direction: column;
	    padding-left: 10px;  
     }
    
   .priorities {
    display: grid;
    grid-template-columns: 25px auto;
    gap: 0px;
  }
  .priorities h2 {
     grid-column:1 / span 2;
     border-bottom: 1px solid var(--outline);
   }
  .priorities .icon {
    grid-column:1;
    width: 25px;
    height: 24px;
    border-right: 1px solid var(--outline);
    border-bottom: 1px solid var(--outline);
  }

  .priorities .content {
     grid-column:2;
     padding: 0px;
     border-bottom: 1px solid var(--outline);
     font-size: 0.9em;
     height: 24px;
     width: auto;
  }

  .weekly-plan {
   display: grid;
    grid-template-columns: 25px auto;
    grid-template-rows: 22px repeat(7, 1fr);
    gap: 0px;
    height: 100%;
   }

  .weekly-plan h2 {
     grid-column:1 / span 2;
     border-bottom: 1px solid var(--outline);
   }

  .weekly-plan .day {
    grid-column:1;
    width: 25px;
    height: auto;
    border-right: 1px solid var(--outline);
    border-bottom: 1px solid var(--outline);
    background-color: var(--outline);
    writing-mode: vertical-rl; /* Vertical text from bottom to top */
    transform: rotate(180deg); /* Rotate text to start from bottom */
    align-items: center; /* Align text to the bottom of the cell */
    justify-content: center; /* Center text horizontally */
  }

  .weekly-plan .content {
     grid-column:2;
     padding: 0px;
     border-bottom: 1px solid var(--outline);
     font-size: 0.9em;
     height: autopx;
     width: auto;
  }

  .notes, .schedule {
    flex: 1;
    display: grid;
    gap: 2px;
  }

  .priorities h2, .notes h2, .schedule h2, .weekly-plan h2 {
    margin-top: 5px;
    font-size: 1.2em;
    color: #555;
    padding-top: 5px;
  }

.schedule {
	padding-right: 10px;
}
  
    .hour, .lines{
      border-top: 1px solid var(--outline);
      font-size: 0.7em;
      height: 22px;
      padding-top: 3px;
    }
    .half-hour {
      height: 22px;
      border-top: 1px dashed var(--outline);
    }
</style>
