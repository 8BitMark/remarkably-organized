<script>
  export let count = 10;
</script>

<div class="container">
    <div class="left-side">
      <div class="notes">
        <h2>Quarterly Goals</h2>
        <div class="lines">Objective...</div>
        {#each Array(3) as _, i}
          <div class="lines"></div>
        {/each}
        <div class="lines">Key Results</div>
        {#each Array(6) as _, i}
          <div class="kr-lines">{i+1}.

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
          </div>
   <div class="kr-midlines">  </div>
        {/each}
     
      <div class="priorities">
   <div class="lines" style="grid-column:1 / span 3;">Action Plan</div>
        {#each Array(12) as _, i}
	    <div class="icon"></div>
	    <div class="content"></div>
      <div class="date">/&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/</div>
        {/each}
	</div>


      </div>
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
    font-size: 1.2em;
    color: var(--text-high);
    text-transform: uppercase;
    padding-top: 5px;
    letter-spacing: 1.5px;
    border-bottom: 1.5px solid var(--outline);
  }
  .lines, .kr-lines, .kr-midline{
      border-bottom: 1px solid var(--outline-high);
      color: var(--text-high);
      text-transform: uppercase;
      font-size: 0.7em;
      height: 24px;
      padding-top: 2px;
    }

  .kr-lines { display: flex; align-items: flex-start;
      height: 25px; }

.kr-midline {
  width:50%;
  }

  .star {
    width: 25px;
    height: 25px;
    fill: none;
    stroke: black;
    stroke-width: 1;
    margin: 2px;
  }
  .progress-bar {
    width: 45%;
    height: 20px;
    background-color: #e0e0e0;
    border-radius: 8px;
    position: relative;
  }

  .progress-bar::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    color: white;
    width: 0%;
    border-radius: 8px 0 0 8px;
    transition: width 0.3s;
  }

  .marker {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 2px;
    height: 20px;
    background-color: white;
  }

  .marker:nth-child(1) { left: 10%; }
  .marker:nth-child(2) { left: 20%; }
  .marker:nth-child(3) { left: 30%; }
  .marker:nth-child(4) { left: 40%; }
  .marker:nth-child(5) { left: 50%; }
  .marker:nth-child(6) { left: 60%; }
  .marker:nth-child(7) { left: 70%; }
  .marker:nth-child(8) { left: 80%; }
  .marker:nth-child(9) { left: 90%; }

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
     font-size: 1.2em;
     height: 24px;
     width: auto;
  }
</style>
