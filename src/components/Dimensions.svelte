<script>
  export let gameState;
  export let rows;
  export let cols;

  const activeBtn = { rows: 0, cols: 0 };

  let customRows = null;
  let customCols = null;

  let invalidInput = false;

  function setActiveBtn(rows, cols) {
    // reset custom fields
    customRows.value = "";
    customCols.value = "";

    activeBtn.rows = rows;
    activeBtn.cols = cols;
  }

  function setCustomRows(rows) {
    if(!rows.match(/\d|^$/)) {
      invalidInput = true;
    } else {
      invalidInput = false;
    }

    if(!invalidInput && rows != "") {
      activeBtn.rows = parseInt(rows);
    } else {
      activeBtn.rows = 0;
    }
  }

  function setCustomCols(cols) {
    if(!cols.match(/\d|^$/)) {
      invalidInput = true;
    } else {
      invalidInput = false;
    }

    if(!invalidInput && cols != "") {
      activeBtn.cols = parseInt(cols);
    } else {
      activeBtn.cols = 0;
    }
  }

  function handleGo() {
    if(activeBtn.rows === 0 || activeBtn.cols === 0) {
      invalidInput = true;
    }
    
    if(!invalidInput) {
      rows = activeBtn.rows;
      cols = activeBtn.cols;
      gameState = "GameRunning";
    }
  }
</script>

<div class="container">
  <form on:submit={handleGo}>
    <button type="button" on:click={() => setActiveBtn(3, 3)} class:active-btn = {activeBtn.rows === 3 && activeBtn.cols === 3}>3 x 3</button>
    <button type="button" on:click={() => setActiveBtn(6, 6)} class:active-btn = {activeBtn.rows === 6 && activeBtn.cols === 6}>6 x 6</button>
    <button type="button" on:click={() => setActiveBtn(9, 9)} class:active-btn = {activeBtn.rows === 9 && activeBtn.cols === 9}>9 x 9</button>
    <div class="custom-input-container">
      <input type="text" bind:this={customRows} on:input={(e) => setCustomRows(e.target.value)}>
      <p>x</p>
      <input type="text" bind:this={customCols} on:input={(e) => setCustomCols(e.target.value)}>
    </div>
    <button class="btn" on:click={handleGo}>Go</button>
  </form>
  {#if invalidInput}
    <p class="invalid-text">Invalid Input!</p>
  {/if}
</div>

<style>
  .container {
    display: grid;
    gap: 1rem;
    text-align: center;
  }

  div * {
    font-size: 2rem;
  }

  form * {
    margin: 10px auto;
  }

  button {
    width: 100%;
    display: block;
    border: none;
    background-color: #000;
    color: #fff;
    padding: 1rem;
    border-radius: 12px;
    cursor: pointer;
  }

  button:hover {
    background-color: rgb(60, 60, 60);
  }

  .active-btn {
    background-color: rgb(60, 60, 60);
  }

  .custom-input-container * {
    display: block;
    width: 100%;
  }

  input[type="text"] {
    border-radius: 12px;
    text-align: center;
    padding: 1rem;
  }
  
  .invalid-text {
    color: red;
  }
</style>