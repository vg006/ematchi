<div class="modal-bg">
    <div class="">
        <slot />
    </div>
</div>

<style>
    .modal-bg {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        backdrop-filter: blur(10px);
        display: grid;
        place-items: center;
        z-index: 999;
    }
</style>