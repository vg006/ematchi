<script lang="ts">
    import Game from "./Game.svelte";
    import '../styles.css';
    import Modal from "./Modal.svelte";
    import {levels} from "./levels";
    import {confetti} from "@neoconfetti/svelte";

    let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = 'waiting';
    let game: Game;
</script>

<Game
    bind:this={game}
    on:play={() => {
        state = 'playing';
    }}
    on:pause={() => {
        state = 'paused';
    }}
    on:win={() => {
        state = 'won';
    }}
    on:lose={() => {
        state = 'lost';
    }}
/>

{#if state !== 'playing'}
    <Modal>
        <header>
            <h1>e<span>match</span>i</h1>
            <p>the emoji matching game</p>

            {#if state === 'won' || state === 'lost'}
                <p>You {state} the game</p>
            {:else if state === 'paused'}
                <p>Game paused</p>
            {:else if state === 'waiting'}
                <p>Choose a level</p>
            {/if}
        </header>

        <div class="buttons">
            {#if state === 'paused'}
                <button on:click={() => state = 'playing'}>Resume</button>
                <button on:click={() => state = 'lost'}>Quit</button>
            {:else}
                {#each levels as level}
                    <button
                        on:click={() => {
                            game.start(level);
                        }}>{level.label}</button>
                {/each}
            {/if}
        </div>

    </Modal>
{/if}

{#if state === 'won'}
    <div class="confetti" use:confetti={{
        stageWidth: window.innerWidth,
        stageHeight: window.innerHeight,
    }}></div>
{/if}

<style>
    h1 {
        font-size: 4em;
    }
    h1 span {
        color: blueviolet;
    }
    p {
        font-family: Grandstander, serif;
    }
    header {
        text-align: center;
        line-height: 1em;
    }
    .buttons button {
        margin: 0.5em;
        padding: 0.5em 1em;
        font-size: 1em;
        border: none;
        border-radius: 0.5em;
        background-color: blueviolet;
        color: white;
        cursor: pointer;
    }
    .confetti {
        position: fixed;
        top: 30%;
        left: 50%;
        width: 100%;
        height: 100%;
        pointer-events: none;
    }
</style>
