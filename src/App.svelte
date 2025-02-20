<script>
    import Content from "./lib/components/Content.svelte";
    import Header from "./lib/components/Header.svelte";
    import Navbar from "./lib/components/Navbar.svelte";
    import pages from "./lib/util/pages";

    let currPage = $state(pages[1]);
</script>

<svelte:head>
    <title>currPage.title</title>
</svelte:head>

<div class="page">
    <div class="navbar-wrapper">
        <Navbar currPage={currPage} />
    </div>

    <div class="header-wrapper">
        <Header />
    </div>

    <main class="content-wrapper">
        <Content />
    </main>
</div>

<style>
    .page {
        display: flex;
        flex-direction: column;
        height: 100vh;
        max-height: 100vh;
        overflow: hidden;
    }

    .navbar-wrapper, .header-wrapper {
        max-height: 56px;
        flex: 0 0 56px;
        position: relative;
        overflow: hidden;
    }

    .content-wrapper {
        background: var(--dark-bg-color);
        position: relative;
        flex: 1 0 auto;
    }

    @media (max-width: 600px) {
        .header-wrapper {
            display: none;
        }
    }
</style>
