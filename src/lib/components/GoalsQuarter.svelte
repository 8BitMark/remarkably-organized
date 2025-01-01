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
  {#each Array(count) as _, i}
    <svg viewBox="0 0 24 24" class="star">
      <path d="M12 .587l3.688 7.455L24 9.803l-6 5.846 1.417 8.272L12 18.971l-7.417 4.95L6 15.649 0 9.803l8.313-1.76z"></path>
    </svg>
  {/each}      
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

</style>
