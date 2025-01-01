<div class="container">
    <div class="left-side">
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