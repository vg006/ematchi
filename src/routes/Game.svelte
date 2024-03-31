<script lang="ts">
    import Grid from "./Grid.svelte";
    import Found from "./Found.svelte";
    import Countdown from "./Countdown.svelte";
    import {type Level, levels} from "./levels.js";
    import {shuffle} from "./utils.js";
    import {createEventDispatcher, onMount} from "svelte";


    let size: number;
    let grid: string[] = [];
    let found: string[] = [];
    let remaining: number = 0;
    let duration: number = 0;
    let playing: boolean = false;

    const dispatch = createEventDispatcher();
    export function start(level: Level) {
        size = level.size;
        grid = create_grid(level);
        remaining = duration = level.duration;
        resume();
    }
    function resume() {
        playing = true;
        countdown();
        dispatch('play');
    }
    function create_grid(level: Level) {
        const copy = level.emojis.slice();
        const pairs: string[] = [];

        for (let i = 0; i < level.size ** 2 / 2; i++) {
            const index = Math.floor(Math.random() * copy.length);
            const emoji = copy[index];
            copy.splice(index, 1);
            pairs.push(emoji);
        }

        pairs.push(...pairs);
        return shuffle(pairs);
    }

    function countdown() {
        const start = Date.now();
        let remainingAtStart = remaining;

        function loop() {
            if(!playing) return;
            requestAnimationFrame(loop);
            remaining = remainingAtStart - (Date.now() - start);
            if (remaining <= 0) {
                playing = false;
                dispatch('lose');
                found = [];
            }
        }
        loop();
    }
</script>

<div class="game" style="--size: {size}">
    <div class="info">
        <Countdown
            {remaining}
            {duration}
            on:click={ () => {
                playing = false;
                dispatch('pause');
            }}
        />
    </div>
    <div class="grid-container">
        <Grid
                {grid}
                on:found={(e) => {
                    found = [...found, e.detail.emoji];
                    if(found.length === size ** 2 / 2) {
                        dispatch('win');
                        found = [];
                    }
                }}
                {found}
        />
    </div>
    <div class="info">
        <Found {found} />
    </div>
</div>

<style>
    .game {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100%;
        font-size: min(1vmin, 0.5rem);
    }

    .info {
        width: 80vmin;
        height: 10vmin;
    }

    .grid-container {
        width: 80vmin;
        height: 70vmin;
    }
</style>

