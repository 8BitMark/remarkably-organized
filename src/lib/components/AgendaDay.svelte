  <div class="container">
    <div class="left-side">
      <div class="priorities">
        <h2>Priorities</h2>
        {#each Array(8) as _, i}
	    <div class="icon"></div>
	    <div class="content"></div>
        {/each}
	<div class="lines"></div>
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
          {(i + 6) % 12 === 0 ? 12 : (i + 6) % 12} {i < 6 ? 'AM' : 'PM'}
        </div>
        <div class="half-hour"></div>
        {/each}
	<div class="hour"></div>
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
	    padding-left: 5px; 
	    padding-right: 5px; 
     }
    
   .priorities {
    display: grid;
    grid-template-columns: 20px auto;
    gap: 2px;
  }
  .priorities .icon {
    width: 22px;
    height: 22px;
    border-right: 1px solid #000;
    border-top: 1px solid var(--outline);
  }
  .priorities .content {
    padding-left: 5px;
     border-top: 1px solid var(--outline);
     font-size: 0.9em;
      height: 22px;
  }
  
  .notes, .schedule {
    flex: 1;
    display: grid;
    gap: 2px;
  }
  .priorities h2, .notes h2, .schedule h2 {
    margin-top: 0;
    font-size: 1.2em;
    color: #555;
    padding-top: 5px;
  }
    
  
    .hour, .lines{
      border-top: 1px solid var(--outline);
      font-size: 0.9em;
      height: 22px;
      padding: 0px;
    }
    .half-hour {
      height: 22px;
      border-top: 1px dashed var(--outline);
    }
</style>
