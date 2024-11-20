  <div class="container">
    <div class="left-side">
      <div class="priorities">
        <h2>Priorities</h2>
        <ul>
          {#each Array(8) as _, i}
          <li>
            <input type="checkbox" id={`priority-${i}`} />
          </li>
          {/each}
        </ul>
      </div>
      <div class="notes">
        <h2>Notes</h2>
        <ul>
          {#each Array(26) as _, i}
          <li>&nbsp;</li>
          {/each}
        </ul>
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
      grid-template-columns: 60% 40%;
      grid-template-rows: repeat(17, 1fr);
      gap: 5px;
      height: 100%;
    }
    .left-side, .right-side {
      display: flex;
      flex-direction: column;
	  padding-left: 5px; 
	  padding-right: 5px; 
     }
    .date {
      margin-bottom: 20px;
      font-size: 1.5em;
      font-weight: bold;
    }
    .priorities, .notes, .schedule {
      flex: 1;
    }
    .priorities h2, .notes h2, .schedule h2 {
      margin-top: 0;
      font-size: 1.2em;
      color: #555;
      padding-top: 5px;
    }
    .priorities ul, .notes ul {
      list-style-type: none;
      padding: 0;
    }
    .priorities li, .notes li {
      border-bottom: 1px solid darkgrey;
      height: 22px;
    }
    .notes ul {
      /* height: calc(100% - 70px); /* Adjust for the header height */
    }
    .notes li::after {
      display: block;
      border-bottom: 1px solid darkgrey;
    }
    .schedule {
      display: grid;
      gap: 2px;
    }
    .hour{
      border-top: 1px solid darkgrey;
      height: 22px;
      padding: 0px;
    }
    .half-hour {
      height: 22px;
      border-top: 1px dashed lightgrey;
    }
</style>
