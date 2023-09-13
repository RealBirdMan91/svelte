<script lang="ts">
	import Game from "../components/Game.svelte";
	import Modal from "../components/Modal.svelte";
    import { levels } from '../lib/levels';
    import "../styles.css"

    let state: "waiting" | "playing" | "won" | "paused" | "lost" = "waiting";
    let game: Game;
</script>


<Game  bind:this={game} on:play = {
    () => {
        state = "playing";
    }
}/>

{#if state !== 'playing'}
		<Modal>
			<header>
				<h1>e<span>match</span>i</h1>
				<p>the emoji matching game</p>
			</header>

			{#if state === 'won' || state === 'lost'}
				<p>you {state}! play again?</p>
			{:else if state === 'paused'}
				<p>game paused</p>
			{:else if state === 'waiting'}
				<p>choose a level:</p>
                
			{/if}

			<div class="buttons">
				{#if state === 'paused'}
					<button on:click={() => {}}>resume</button>
					<button on:click={() => (state = 'waiting')}> quit </button>
                   {:else}
                   {#each levels as level}
                   <button on:click={() => game.start(level)}> {level.label} </button>
               {/each}
				{/if}
			</div>
		</Modal>
	{/if}

    <style>
        main {
            text-align: center;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
    
        header {
            font-size: min(5vw, 2rem);
            font-family: Grandstander;
        }
    
        h1 {
            font-size: 4em;
            margin: 0;
            height: 1em;
        }
    
        h1 span {
            color: var(--accent);
        }
    
        p {
            margin: 0 0 1em 0;
        }
    
        .buttons {
            display: flex;
            justify-content: center;
            gap: 0.5em;
        }
    
        button {
            background: var(--accent);
            color: white;
            font-size: inherit;
            font-family: inherit;
            border: none;
            padding: 1em;
            border-radius: 0.5em;
        }
    </style>