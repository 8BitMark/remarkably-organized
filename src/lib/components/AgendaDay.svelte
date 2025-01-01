  <div class="container">
    <div class="left-side">
      <div class="priorities">
        <h2>Priorities</h2>
        {#each Array(8) as _, i}
	    <div class="icon"></div>
	    <div class="content"></div>
        {/each}
	</div>
      <div class="notes">
        <h2>Notes</h2>
        {#each Array(26) as _, i}
          <div class="lines"></div>
        {/each}
      </div>
    </div>
    <div class="right-side">
      <div class="schedule">
        <h2>Schedule</h2>
        {#each Array(17) as _, i (i)}
        <div class="hour">
          {(i + 6)}
<!--% 12 === 0 ? 12 : (i + 6) % 12} {i < 6 ? 'AM' : 'PM'}-->
        </div>
        <div class="half-hour"></div>
        {/each}
	<div class="hour"></div>
	<div class="half-hour"></div>
      </div>
    </div>
</div>



<style lang="scss">
    .container {
      display: grid;
      grid-template-columns: 55% 45%;
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

   .right {
    padding-right: 5px;
    }

   .priorities {
    display: grid;
    grid-template-columns: 25px auto;
    gap: 0px;
  }
  .priorities h2 {
     grid-column:1 / span 2;
     border-bottom: 1.5px solid var(--outline);
     text-transform: uppercase;
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
  
  .notes, .schedule {
    flex: 1;
    display: grid;
    gap: 2px;
  }
  .priorities h2, .notes h2, .schedule h2 {
    margin-top: 5px;
    font-size: 1.2em;
    color: var(--text-high);
    text-transform: uppercase;
    padding-top: 5px;
    letter-spacing: 1.5px;
    border-bottom: 1.5px solid var(--outline);
  }

.schedule {
	padding-right: 10px;
}
  
    .hour {
      border-bottom: 1px dashed var(--outline-high);
      color: var(--text-high);
      font-size: 0.7em;
      height: 22px;
      padding-top: 2px;
    }

    .lines {
      border-bottom: 1px solid var(--outline-high);
      color: var(--text-high);
      font-size: 0.7em;
      height: 24px;
      padding-top: 2px;
    }

    .half-hour {
      font-size: 0.7em;
      height: 22px;
      padding-top: 2px;
      color: var(--text-high);
      border-bottom: 1px solid var(--outline-high);
    }
</style>
