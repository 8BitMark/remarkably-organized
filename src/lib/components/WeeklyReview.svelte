<div class="container">
    <div class="left-side">
      <div class="notes">
        <h2>Weekly Review</h2>
        <div class="lines">What were my biggest accomplishments this week...</div>
        {#each Array(6) as _, i}
          <div class="lines"></div>
        {/each}
        <div class="lines">What worked...</div>
        {#each Array(6) as _, i}
          <div class="lines"></div>
        {/each}
        <div class="lines">What didn't work...</div>
        {#each Array(6) as _, i}
          <div class="lines"></div>
        {/each}
        <div class="lines">How can next week be better...</div>
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
  .lines {
      border-bottom: 1px solid var(--outline-high);
      color: var(--text-high);
      text-transform: uppercase;
      font-size: 0.7em;
      height: 24px;
      padding-top: 2px;
    }
</style>