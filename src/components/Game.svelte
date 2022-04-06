<script>
  export let rows = 3;
  export let cols = 3;
  
  let turn = (Math.random() < 0.5) ? "P1" : "P2";

  const winningCombos = generateWinningCombos();
  let playerOneTurns = [];
  let playerTwoTurns = [];

  let playerOneWon = false;
  let playerTwoWon = false;
  let draw = false;

  function handleClick(e) {
    if(turn === "P1") {
      turn = "P2";

      playerOneTurns.push(parseInt(e.target.getAttribute("index")));
      e.target.src = "./images/check-mark.png";

      if(checkWin("P1")) {
        playerOneWon = true;
      }
    } else {
      turn = "P1";

      playerTwoTurns.push(parseInt(e.target.getAttribute("index")));
      e.target.src = "./images/cross-mark.png";

      if(checkWin("P2")) {
        playerTwoWon = true;
      }
    }

    if(checkDraw()) {
      draw = true;
    }
  }

  function checkWin(player) {
    for(let i of winningCombos) {
      let playerWon = true;

      for(let j of i) {
        if(player === "P1") {
          if(!playerOneTurns.includes(j)) {
            playerWon = false;
          }
        } else {
          if(!playerTwoTurns.includes(j)) {
            playerWon = false;
          }
        }
      }

      if(playerWon) {
        return playerWon;
      }
    }

    return false;
  }

  function checkDraw() {
    let draw = true;
    let allIndices = [];
    let index = 0;

    for(let i = 0; i < rows; i++) {
      for(let j = 0; j < cols; j++) {
        allIndices.push(index);
        index++;
      }
    }

    allIndices.forEach((i) => {
      if(!playerOneTurns.includes(i) && !playerTwoTurns.includes(i)) {
        draw = false;
      }
    });

    return draw;
  }

  function generateWinningCombos() {
    let combos = [];

    let index = 0;

    for(let i = 0; i < cols; i++) {
      let combo = [];

      for(let j = 0; j < cols; j++) {
        combo.push(index);

        index++;
      }

      combos.push(combo);
    }

    index = 0;

    for(let i = 0; i < cols; i++) {
      let combo = [];

      for(let j = 0; j < rows; j++) {
        combo.push(index);

        index += cols;
      }

      index = i + 1;

      combos.push(combo);
    }

    // last two combos
    if(rows == cols) {
      let combo = [];
      index = 0;

      for(let i = 0; i < rows; i++) {
        combo.push(index);

        index += (rows + 1);
      }

      combos.push(combo);

      combo = [];
      index = rows - 1;

      for(let i = 0; i < rows; i++) {
        combo.push(index);

        index += (rows - 1);
      }

      combos.push(combo);
    }

    return combos;
  }

  function handlePlayAgain() {
    location.reload();
  }
</script>

{#if turn === "P1"}
  <h2 class="turn-text">Player 1's turn</h2> 
{:else}
  <h2 class="turn-text">Player 2's turn</h2>
{/if}
<div class="container" style={`grid-template-columns: repeat(${cols}, min-content)`}>
  {#each Array(rows * cols).fill().map((_, idx) => 0 + idx) as i}
    <div class="box">
      <img alt="" index={i} on:click|once={handleClick}>
    </div>
  {/each}
</div>
{#if playerOneWon}
  <div class="overlay">
    <h1>Player One Won!</h1>
    <button on:click={handlePlayAgain}>Play Again!</button>
  </div>
{:else if playerTwoWon}
  <div class="overlay">
    <h1>Player Two Won!</h1>
    <button on:click={handlePlayAgain}>Play Again!</button>
  </div>
{:else if draw}
  <div class="overlay">
    <h1>Draw!</h1>
    <button on:click={handlePlayAgain}>Play Again!</button>
  </div>
{/if}

<style>
  .container {
    display: grid;
    gap: 10px;
    padding: 1rem;
    overflow: scroll;
    max-height: 100vh;
    max-width: 100vw;
  }

  .box {
    width: 50px;
    height: 50px;
    border: 1px solid #000;
    background: #fff;
  }

  .box img {
    height: 100%;
    width: 100%;
  }

  .overlay {
    position: fixed;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.6);
    color: #fff;
    top: 0;
    left: 0;
  }

  .overlay * {
    margin: 1rem;
  }

  button {
    border: none;
    background-color: #000;
    color: #fff;
    padding: 1rem;
    border-radius: 12px;
    cursor: pointer;
    font-size: 1.5rem;
  }

  button:hover {
    background-color: rgb(60, 60, 60);
  }

  .turn-text {
    padding: 1rem;
  }
</style>