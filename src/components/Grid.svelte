<script lang="ts">
	import { createEventDispatcher } from "svelte";
	import Square from "./Square.svelte";
    
    export let grid: string[];
    export let found: string[];
    

    let a: number = -1;
    let b: number = -1;
    const dispatch = createEventDispatcher<{found: {emoji: string}}>();

    
    $: if(a !== -1 && b !== -1){
        if(grid[a] === grid[b]){
            dispatch("found", {emoji: grid[a]});
        }
       setTimeout(() => {
            a = -1;
            b = -1;
        }, 1000);
    }

</script>


<div class="grid">
    {#each grid as emoji, i}
        <Square 
        {emoji} 
        on:click={() => {
            if(a === -1 && b === -1){
                a = i;
                return;
            }
            if(a !== -1 && a !== i && b === -1){
                b = i;
                return;
            }

        }}
        selected={i === a || i === b}
        found={found.includes(emoji)}
        />
    {/each}
</div>



<style>
    .grid{
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: repeat(4, 1fr);
        height: 100%;
        gap: 0.5rem;
        perspective: 100vw;
    }
</style>