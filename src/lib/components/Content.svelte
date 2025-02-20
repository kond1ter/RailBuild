<script>
    import { fly } from "svelte/transition";
    import Map from "./Map.svelte";
    import VideoBar from "./VideoBar.svelte";
    import Video from "./Video.svelte";
	import "../../assets/css/player.css";

    let contentState = $state({
        mapExpanded: true,
        videoBarVisible: false,
        currentVideo: null,
        videoPlaying: false
    })

    let mapWrapperClass = $derived.by(() => {
        return contentState.mapExpanded
            ? "map-wrapper_map-mode"
            : "map-wrapper_video-mode";
    })
</script>

<button class="sidebar-button" aria-label="Toggle sidebar">
    <i class="bi-chevron-double-right"></i>
</button>

<button class="elements-button" aria-label="Toggle elements">
    <i class="bi-funnel-fill"></i>
</button>

<div class="map-wrapper {mapWrapperClass} {contentState.videoPlaying ? 'map-wrapper_video-playing' : ''}">
    <Map bind:contentState={contentState} />
</div>

<button 
    onclick={() => contentState.videoBarVisible = true}
    class="other-dates-bttn {contentState.videoPlaying ? 'other-dates-bttn_video-playing' : ''}">
	<p>Другие даты</p>
	<i class="bi-chevron-up"></i>
</button>

<div class="video-wrapper">
    <Video bind:contentState={contentState} />
</div>

{#if contentState.videoBarVisible}
    <div transition:fly={{ y: 200, duration: 200 }} class="video-bar-wrapper">
        <VideoBar bind:contentState={contentState} />
    </div>
{/if}

<style>
    .sidebar-button, .elements-button {
        z-index: 4;
        display: none;
        position: absolute;
        border: 1px solid var(--outline-color);
        background: var(--light-bg-color);
        border-radius: 8px;
        padding: 4px 12px;
    }

    .sidebar-button {
        color: var(--dark-text-color);
        font-size: 1em;
        top: 8px;
        left: 8px;
    }

    .elements-button {
        color: var(--accent-color);
        font-size: 1em;
        top: 8px;
        right: 8px;
    }

    .map-wrapper {
        z-index: 3;
        position: absolute;
        transition: 0.3s;
        transform-origin: bottom left;
        width: 100%;
        height: 100%;
    }

    .map-wrapper_map-mode {
        bottom: 0;
        left: 0;
    }

    .map-wrapper_video-mode {
        bottom: 70px;
        left: 25px;
        transform: scale(0.25);
    }

    .video-wrapper {
        bottom: 0;
        left: 0;
        z-index: 1;
        position: absolute;
        width: 100%;
        height: 100%;
        background: #000;
    }

    .video-bar-wrapper {
        position: absolute;
        z-index: 4;
        bottom: 0;
        left: 0;
        width: 100%;
    }

    .other-dates-bttn {
        position: absolute;
        z-index: 2;
        right: 25px;
        bottom: 70px;
        background: var(--tonal-light-bg-color);
        color: var(--light-text-color);
        display: inline-flex;
        gap: 8px;
        padding: 8px;
        border-radius: 6px;
        transition: 0.3s;
    }

    .other-dates-bttn:hover {
        background: var(--hover-color);
    }

    .map-wrapper_video-playing {
        opacity: 0;
        pointer-events: none;
    }

    .other-dates-bttn_video-playing {
        opacity: 0;
        pointer-events: none;
    }

    @media (max-width: 600px) {
        .sidebar-button, .elements-button {
            display: block;
        }
    }
</style>
