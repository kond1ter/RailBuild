<button aria-label="Toggle" class="burger-button">
    <div class="burger-button__bar burger-button__bar-1"></div>
    <div class="burger-button__bar burger-button__bar-2"></div>
    <div class="burger-button__bar burger-button__bar-3"></div>
</button>

<style>
    .burger-button {
        position: relative;
        width: 100%;
        height: 100%;
        background: transparent;
    }

    .burger-button__bar {
        width: 100%;
        height: 0;
        border: 1px solid var(--accent-color);
        position: absolute;
    }

    .burger-button__bar-1 {
        top: 30%;
    }

    .burger-button__bar-2 {
        top: 50%;
    }

    .burger-button__bar-3 {
        top: 70%;
    }
</style>