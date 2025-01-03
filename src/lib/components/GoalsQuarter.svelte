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
        {/each}
        <div class="lines">Action Plan</div>
        {#each Array(6) as _, i}
          <div class="lines"></div>
        {/each}
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
  .lines, .kr-lines{
      border-bottom: 1px solid var(--outline-high);
      color: var(--text-high);
      text-transform: uppercase;
      font-size: 0.7em;
      height: 24px;
      padding-top: 2px;
    }

  .kr-lines { display: flex; align-items: flex-start;
      height: 35px; }

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
    border-radius: 6px;

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
</style>
