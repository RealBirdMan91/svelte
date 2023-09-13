<script lang="ts">
    export let found: string[] = [];
    console.log(found);
</script>

<div class="found">
    {#each found as emoji}
        <span>{emoji}</span>
    {/each }
</div>

<style>
    .found{
        display: flex;
        justify-content: center;
        align-items: center;
        background: purple;
        font-size: min(1vmin, 0.3rem);
        gap: 0.5em;
    }
    span{
        font-size: 5em;
    }
</style>