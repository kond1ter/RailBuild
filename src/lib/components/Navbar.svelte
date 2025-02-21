<script>
    import pages from "../util/pages";

    let { currPage } = $props()
</script>

<nav class="navbar">
    <ul class="navbar__list">
        {#each pages as page, i}
            <li class="navbar__item {page.id == currPage.id ? 'navbar__item_selected' : ''}">
                <a href={page.url}>{ page.title }</a>
            </li>

            {#if pages[i + 1]?.id != currPage.id && page.id != currPage.id}
                <div class="navbar__separator"></div>
            {/if}
        {/each}
    </ul>
</nav>

<style>
    .navbar {
        width: 100%;
        height: 100%;
        background: var(--accent-bg-color);
        overflow-x: auto;
    }

    .navbar__list {
        height: 100%;
        width: fit-content;
        display: inline-flex;
        align-items: center;
    }

    .navbar__item {
        height: 100%;
    }

    .navbar__item a {
        text-wrap: nowrap;
        height: 100%;
        display: flex;
        color: var(--dark-text-color);
        align-items: center;
        padding: 0 24px;
        font-family: "Raleway Regular";
        font-size: 1em;
    }

    .navbar__separator {
        height: 60%;
        border: 1px solid var(--separator-color);
    }

    .navbar__item_selected {
        background: var(--light-bg-color);
    }
</style>
