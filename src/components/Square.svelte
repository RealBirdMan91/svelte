<script lang="ts">
    export let emoji: string;
    export let selected: boolean = false; 
    export let found: boolean = false;
</script>

<div class="square" class:flipped={selected }>
    {#if !found}
    <button on:click/>
    <span>{emoji}</span>
    {/if}
</div>


<style>
    .square{
        display: flex;
        justify-content: center;
        align-items: center;
        transform-style: preserve-3d;
        transition: transform 0.5s;
        transform: rotateY(180deg);
    }

    button {
        width: 100%;
        height: 100%;
        position: absolute;
    }
    .flipped{
        transform: rotateY(0deg);
    }

    span{
        font-size: 3em;
        backface-visibility: hidden;
        }
</style>