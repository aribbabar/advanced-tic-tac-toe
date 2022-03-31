<script>
	import Game from "./components/Game.svelte";
	import Dimensions from "./components/Dimensions.svelte";

	let gameState = "PickDimensions";
	let rows = 3;
	let cols = 3;
</script>

<header>
	<h1>Advanced Tic Tac Toe</h1>
</header>
<main>
	{#if gameState === "PickDimensions"}
		<Dimensions bind:gameState = {gameState} bind:rows = {rows} bind:cols = {cols} />
	{:else if gameState === "GameRunning"}
		<Game {rows} {cols} />
	{/if}
</main>
<footer>
	<h3>© All rights reserved | Arib Farooqui</h3>
</footer>

<style>
	header {
		padding: 2rem;
		background: #e6e6fa;
		min-height: 20vh;
		display: flex;
		align-items: center;
	}

	h1 {
		display: inline-block;
		font-size: 4rem;
		border-bottom: 4px solid #000;
	}

	main {
		padding: 2rem;
		background: #f0f8ff;
		min-height: 80vh;
	}

	@media (min-width: 1000px) {
		main {
			display: flex;
			align-items: center;
			justify-content: center;
		}
	}

	footer {
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #e6e6fa;
		min-height: 20vh;
	}
</style>