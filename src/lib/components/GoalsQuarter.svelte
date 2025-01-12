<script>
  export let count = 10;
</script>

<div class="container">
      <div class="notes">
        <h2>Quarterly Goals</h2>
        <div class="lines">Objective</div>
        {#each Array(4) as _, i}
          <div class="lines"></div>
        {/each}
      </div>

 <div class="results">
      <div class="lines" style="grid-column:1 / span 2;">Key Results</div>
      {#each Array(6) as _, i}
	    	<div class="krlines" style="grid-column:1;">{i+1}.</div>
   			<div class="krlines" style="grid-column:1;"></div>
<div class="progress-bar">
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
  <div class="marker"></div>
</div>
      {/each}
</div>

<div class="priorities">
   <div class="lines" style="grid-column:1 / span 2;">Action Plan</div>
   <div class="lines" style="grid-column:3;">Due Date</div>
   
   {#each Array(12) as _, i}
	    <div class="icon"></div>
	    <div class="content"></div>
      <div class="date"></div>
   {/each}
</div>
</div>

<style lang="scss">
    .container {
      display: grid;
      grid-template-columns: 100%;
      grid-template-rows: repeat(17, 1fr);
      gap: 5px;
      padding-left: 10px;
      padding-right: 10px;
      height: 98%;
      widgth: 95%;
    }


  .notes {
    flex: 1;
    display: grid;
    gap: 2px;
  }
  .notes h2 {
    margin-top: 5px;
    font-size: 0.95em;
    color: var(--text-high);
    text-transform: uppercase;
    padding-top: 5px;
    letter-spacing: 1.5px;
    border-bottom: 1.5px solid var(--outline);
  }

  .lines {
      border-bottom: 1px solid var(--outline-high);
      color: var(--text-high);
      text-transform: uppercase;
      font-size: 0.7em;
      height: 24px;
      padding-top: 2px;
    }

   .results{
	    display: grid;
      	grid-template-columns: 65% 35%; /* Two columns with auto widths */
      gap: 0px;
    }
  .results .krlines {
     grid-column:1;
     padding: 0px;
     border-bottom: 1px solid var(--outline-high);
     font-size: 0.9em;
     height: 24px;
     width: auto;
  }
  
.star {
    width: 25px;
    height: 25px;
    fill: none;
    stroke: black;
    stroke-width: 1;
    margin: 2px;
  }

  .priorities {
    display: grid;
    grid-template-columns: 25px auto 150px;
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
  .priorities .date {
     grid-column:3;
     padding: 0px;
    text-align: center;
     border-left: 1px solid var(--outline-high);
     border-bottom: 1px solid var(--outline-high);
     font-size: 0.95em;
     height: 24px;
     width: auto;
  }
</style>
