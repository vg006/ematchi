<script lang="ts">
    import { send } from './transitions.ts';
    export let emoji: string;
    export let selected: boolean;
    export let found: boolean;
    export let group: 'a' | 'b';
</script>

<div class="square" class:flipped={selected || found}>
    <button on:click />
    <div class="background" />

    {#if !found}
        <span
        out:send={{key: `${emoji}:${group}`}}>{emoji}</span>
    {/if}
</div>

<style>
    .square {
        display: flex;
        justify-content: center;
        align-items: center;
        transform-style: preserve-3d;
        transition: transform 0.5s;
    }

    .flipped {
        transform: rotateY(180deg);
    }

    .background {
        position: absolute;
        background: #fff;
        border: 0.5em solid #eee;
        border-radius: 2em;
        transform: rotateY(180deg);
        backface-visibility: hidden;
        width: 100%;
        height: 100%;
    }

    button {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        background: #eeeeee;
        border: none;
        border-radius: 2em;
        font-size: inherit;
    }

    span {
        font-size: 5em;
        pointer-events: none;
        transform: rotateY(180deg);
        backface-visibility: hidden;
    }
</style>