<script>
    import { onMount } from "svelte";
    import { getVideosData } from "../util/mockapi";
    import { fade } from "svelte/transition";

    let { contentState = $bindable() } = $props();
    let videosDataPromise = $state(getVideosData());
    let thumbnail = $state(null);
    let thumbnailsList = $state(null);
    let scroll = $state(0);

    const scrollRight = () => {
        if (thumbnail == null || thumbnailsList == null) return;
        const style = window.getComputedStyle(thumbnail);
        const scrollDelta = thumbnail.offsetWidth + parseInt(style.marginLeft) * 2;

        scroll = scroll - scrollDelta < -thumbnailsList.scrollWidth + thumbnailsList.offsetWidth
            ? -thumbnailsList.scrollWidth + thumbnailsList.offsetWidth
            : scroll - scrollDelta;
    }

    const scrollLeft = () => {
        if (thumbnail == null || thumbnailsList == null) return;
        const style = window.getComputedStyle(thumbnail);
        const scrollDelta = thumbnail.offsetWidth + parseInt(style.marginLeft) * 2;

        scroll = scroll + scrollDelta > 0 
            ? 0 
            : scroll + scrollDelta;
    }

    const handleClick = (videoData) => {
        contentState.mapExpanded = false;
        contentState.videoBarVisible = false;
        contentState.currentVideo = videoData;
    }

    const onWindowResize = () => {
        scroll = 0;
    }

    onMount(() => {
        window.addEventListener("resize", onWindowResize);
        return () => window.removeEventListener("resize", onWindowResize);
    })
</script>

<div class="video-bar">
    {#await videosDataPromise}
        <div class="video-bar__thumbnails-list">
            {#each Array.from({ length: 10 })}
                <div class="video-bar__thumbnail-wrapper">
                </div>
            {/each}
        </div>
    {:then videosData} 
        <button transition:fade={{duration: 300}} class="video-bar__bttn-left" aria-label="Scroll left" onclick={scrollLeft}>
            <i class="bi-chevron-left"></i>
        </button>

        <button transition:fade={{duration: 300}} class="video-bar__bttn-right" aria-label="Scroll right" onclick={scrollRight}>
            <i class="bi-chevron-right"></i>
        </button>

        <div 
            class="video-bar__thumbnails-list" 
            bind:this={thumbnailsList} 
            style="transform: translateX({scroll}px);"
        >
            {#each videosData as data (data.id)}
                {#if data.id == contentState?.currentVideo?.id}
                    <button class="video-bar__thumbnail-wrapper" bind:this={thumbnail} style="opacity: 0.5;">
                        <div class="video-bar__thumbnail-date-wrapper">{data.formatedDate}</div>
                        <img src={data.thumbUrl} alt="Video thumbnail" class="video-bar__thumbnail">
                    </button>
                {:else}
                    <button class="video-bar__thumbnail-wrapper" onclick={() => handleClick(data)} bind:this={thumbnail}>
                        <div class="video-bar__thumbnail-date-wrapper">{data.formatedDate}</div>
                        <img src={data.thumbUrl} alt="Video thumbnail" class="video-bar__thumbnail">
                    </button>
                {/if}
            {/each}
        </div>
    {/await}
</div>

<style>
    .video-bar {
        background: var(--tonal-light-bg-color);
    }

    .video-bar__thumbnails-list {
        display: flex;
        position: relative;
        transition: 0.5s;
    }

    .video-bar__bttn-left, .video-bar__bttn-right {
        background: var(--tonal-bg-color);
        border-radius: 999px;
        color: var(--light-text-color);
        height: 35px;
        width: 35px;
        position: absolute;
        top: 50%;
        z-index: 5;
        transform: translateY(-50%);
        border: 1px solid var(--outline-color);
    }

    .video-bar__bttn-left {
        left: 6px;
    }

    .video-bar__bttn-right {
        right: 6px;
    }

    .video-bar__thumbnail-wrapper {
        width: min(calc(100% - 32px), 360px);
        height: 240px;
        flex: 0 0 auto;
        position: relative;
        border: 1px solid var(--light-bg-color);
        text-align: left;
        margin: 16px;
        background: var(--tonal-dark-bg-color);
    }

    .video-bar__thumbnail {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .video-bar__thumbnail-date-wrapper {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        background: var(--tonal-dark-bg-color);
        font-family: "Raleway Bold";
        color: var(--light-text-color);
        padding: 8px;
    }
</style>