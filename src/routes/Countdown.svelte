<script lang="ts">
    export let remaining: number;
    export let duration: number;
</script>

<div class="countdown">
    <button on:click>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <title>pause</title>
            <path
                    d="M14,19H18V5H14M6,19H10V5H6V19Z"
                    fill="#999"/>
        </svg>
    </button>
    <div class="duration">
        <div class="remaining" style="--p: {remaining / duration}"/>
    </div>
</div>

<style>
    .countdown {
        display: flex;
        align-items: center;
        gap: 1em;
    }

    .duration {
        flex: 1;
        height: 1em;
        background: #ccc;
    }

    .remaining {
        position: absolute;
        width: calc(var(--p) * 100%);
        height: 100%;
        background: #999;
    }

    button {
        font-size: 8em;
        width: 1em;
        height: 1em;
        background: transparent;
        border: none;
    }

    svg {
        position: absolute;
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
    }
</style>