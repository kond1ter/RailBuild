<script>
	import Plyr from "plyr";
	import { onMount } from "svelte";
	
    let { contentState = $bindable() } = $props();
	let video = $derived(contentState.currentVideo);
	let player;

	onMount(() => {
		player = new Plyr("#player", {
			keyboard: {
				global: true,
			},
		});

		player.on("playing", () => {
			contentState.videoPlaying = true;
			contentState.videoBarVisible = false;
		})

		player.on("pause", () => {
			if (!player.seeking) contentState.videoPlaying = false;
			contentState.videoBarVisible = false;
		})
	})

	$effect(() => {
		video;
		if (video == null) return;

		player.source = {
			type: "video",
			title: video.title,
			poster: video.thumbUrl,
			sources: [
				{
					src: video.videoUrl,
					type: "video/mp4",
					size: 360
				}
			]
		}

		return () => {
			setTimeout(() => {player.stop()}, 300)
		};
	})
</script>

<!-- svelte-ignore a11y_media_has_caption -->
<video 
	controls 
	playsinline
	id="player"
></video>

<style>
	:global(:root) {
		--plyr-color-main: var(--accent-color);
		--plyr-video-background: #000;
	}

	:global(.plyr) {
		width: 100%;
		height: 100%;
	}

    #player {
        width: 100%;
		height: 100%;
    }
</style>