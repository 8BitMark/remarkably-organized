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
<svg width="500" height="50">
  <!-- Define clip-paths for rounded corners -->
  <defs>
    <clipPath id="left-rounded">
      <rect x="0" y="0" width="10" height="50" rx="5" />
    </clipPath>
    <clipPath id="right-rounded">
      <rect x="0" y="0" width="10" height="50" rx="5" />
    </clipPath>
  </defs>
  
  <!-- Leftmost box with left rounded corner -->
  <rect x="0" y="0" width="10" height="50" fill="blue" clip-path="url(#left-rounded)" />
  
  <!-- Middle boxes without rounded corners -->
  <rect x="10" y="0" width="40" height="50" fill="blue" />
  <rect x="50" y="0" width="40" height="50" fill="blue" />
  <rect x="90" y="0" width="40" height="50" fill="blue" />
  <rect x="130" y="0" width="40" height="50" fill="blue" />
  <rect x="170" y="0" width="40" height="50" fill="blue" />
  <rect x="210" y="0" width="40" height="50" fill="blue" />
  <rect x="250" y="0" width="40" height="50" fill="blue" />
  <rect x="290" y="0" width="40" height="50" fill="blue" />
  
  <!-- Rightmost box with right rounded corner -->
  <rect x="330" y="0" width="10" height="50" fill="blue" clip-path="url(#right-rounded)" transform="scale(-1, 1) translate(-10, 0)" />
</svg>
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
