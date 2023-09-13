<script lang="ts">
	import Grid from "./Grid.svelte";
    import {levels} from "../lib/levels";
    import type {Level} from "../lib/levels";
	import Found from "./Found.svelte";
	import Countdown from "./Countdown.svelte";
	import { createEventDispatcher, onMount } from "svelte";


    let size: number;
    let grid: string[] = []
    let found: string[] = [];
    let remaining: number = 0;
    let duration: number = 0;
    let playing: boolean = false;

    const dispatch = createEventDispatcher();


    function create_grid(level: Level){
         const {emojis} = level;
         const pairs: string[] = [];

         for(let i = 0; i < level.size * level.size / 2; i++ ){
            const index = Math.floor(Math.random() * emojis.length);
            const emoji = emojis[index];
            
            emojis.splice(index, 1);
            pairs.push(emoji);
         }
        return [...pairs, ...pairs].sort(() => Math.random() - 0.5)
    }

    function countdown(){
        const start = Date.now();
        let remaining_at_start = remaining;

        function loop(){

            if(!playing) return;
            requestAnimationFrame(loop);
            remaining = remaining_at_start - (Date.now() - start);

            if(remaining <= 0){
                playing = false
                alert("You lost!");
            }
        }
        loop();
    }

    function resume(){
        playing = true;
        countdown();

        dispatch("play");
    }

   export function start(level: Level){
        size = level.size;
        grid = create_grid(level);
        duration = level.duration;
        remaining = level.duration;
       resume();
    }

    onMount(countdown)
</script>


<div class="game">
    <div class="info">
       {#if playing}
            <Countdown duration={duration} remaining={remaining} on:click={()=> {
                //Todo pause game
            }}/>
       {/if}
    </div>
    <div class="grid-container">
        <Grid 
        {grid} 
        on:found={(e)=> {
            found = [...found, e.detail.emoji];
            if(found.length === size * size){
                alert("You won!");
            }
        }}
        {found }/>
    </div>
    <div class="info">
        <Found {found}/>
    </div>
</div>

<style>
    .game{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-size: min(1vmin, 0.3rem);
    }

    .info{
        width: 80em;
        height: 10em;
        background: purple;
    }

    .grid-container{
        width: 80em;
        height: 80em;
        background:teal;
    }
</style>

