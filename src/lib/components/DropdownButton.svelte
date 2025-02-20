<script>
    let { label = "" } = $props();
</script>

<button class="dropdown">
    <p class="dropdown__label">{label}</p>
    <i class="bi-caret-down-fill"></i>
</button>

<style>
    .dropdown {
        background: transparent;
        display: flex;
        align-items: center;
        height: 100%;
        gap: 8px;
        padding: 0 8px;
        transition: 0.3s;
    }

    .dropdown i {
        color: var(--accent-bg-color);
    }

    .dropdown__label {
        font-family: "Raleway Semibold";
        font-size: 1em;
    }
</style>