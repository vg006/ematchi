<script lang="ts">
    import { receive} from "./transitions";
    export let found: string[];
</script>

<div class="found">
    {#each found as emoji (emoji)}
        <div class="pair">
            <span in:receive={{ key: `${emoji}:a` }}> {emoji} </span>
            <span in:receive={{ key: `${emoji}:b` }}> {emoji} </span>
        </div>
    {/each}
</div>

<style>
    .found {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 0.5em;
        height: 100%;
        filter: drop-shadow(0.2em 0.2em 0.5em black);
    }
    .pair {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 8em;
        height: 1em;
        padding: 1em;
        border-radius: 50%;
    }
    span {
        position: absolute;
        font-size: 5em;
        width: 2em;
        height: 100%;
    }
</style>