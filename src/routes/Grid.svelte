<script lang="ts">
    import Square from "./Square.svelte";
    import {createEventDispatcher} from "svelte";
    export let grid: string[];
    export let found: string[];

    const dispatch = createEventDispatcher();

    let a: number = -1;
    let b: number = -1;
    let reset_timeout: number;
</script>

<div class="grid">
    {#each grid as emoji, i}
        <Square
            {emoji}
            on:click={() => {
                if (a === -1 && b === -1) {
                    a = i;
                } else if (b === -1) {
                    b = i;
                    if(grid[a] === grid[b]) {
                        dispatch('found', {emoji});
                    } else {
                        reset_timeout = setTimeout(() => {
                            a = -1;
                            b = -1;
                        }, 1000);
                    }
                } else {
                    a = i;
                    b = -1;
                }
            }}
            selected={a === i || b === i}
            found={found.includes(emoji)}
            group={grid.indexOf(emoji) === i ? 'a' : 'b'}
        />
    {/each}
</div>

<style>
    .grid {
        display: grid;
        grid-template-columns: repeat(var(--size), 1fr);
        grid-template-rows: repeat(var(--size), 1fr);
        grid-gap: 0.5em;
        height: 100%;
        perspective: 100vw;
    }
</style>