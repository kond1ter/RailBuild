<script>
    import { onMount } from 'svelte';
    import { getGeoJson } from '../util/mockapi';
    import maplibregl from 'maplibre-gl';
    import "maplibre-gl/dist/maplibre-gl.css";

    let { contentState = $bindable() } = $props();
    let geoJson = $state(getGeoJson());
    let featureOver = $state(false);
    
    let map;
    let mapContainer;

    const lineLayerId = "line-layer";
    const lineSourceId = "line-source";

    onMount(() => {
        map = new maplibregl.Map({
            container: mapContainer.id,
            style: "https://api.maptiler.com/maps/streets-v2/style.json?key=6XCcjC6WVLlf4xovwI6t",
            center: [36.1289300, 56.7269449],
            zoom: 9.7,
        });

        map.on("load", () => {
            geoJson.then((geo) => initMap(geo));
        })

        return () => map.remove();
    })

    const initMap = (geo) => {
        map.addSource(lineSourceId, {
            type: "geojson",
            data: geo,
        })

        map.addLayer({
            id: lineLayerId,
            source: lineSourceId,
            type: "line",
            paint: {
                "line-color": '#c11',
                "line-width": 2
            }
        })

        map.on("click", (e) => {
            const clickbox = [
                [e.point.x - 5, e.point.y - 5],
                [e.point.x + 5, e.point.y + 5]
            ];

            const selectedFeatures = map.queryRenderedFeatures(clickbox, {
                layers: [lineLayerId]
            });

            if (selectedFeatures.length != 0) {
                if (!contentState.videoBarVisible) contentState.videoBarVisible = true;
            } else {
                if (contentState.videoBarVisible) contentState.videoBarVisible = false;
            }
        })

        map.on("mousemove", (e) => {
            const overbox = [
                [e.point.x - 5, e.point.y - 5],
                [e.point.x + 5, e.point.y + 5]
            ];

            const features = map.queryRenderedFeatures(overbox, {
                layers: [lineLayerId]
            });

            if (features.length != 0) featureOver = true;
            else featureOver = false;
        })
    }

    const expandMap = () => {
        contentState.mapExpanded = true;
        contentState.currentVideo = null;
    }
</script>

<button 
    class="map__expand-bttn 
        {!contentState.mapExpanded && !contentState.videoPlaying ? 'map__expand-bttn_touchable' : ''}" 
    aria-label="Expand map"
    onclick={expandMap}
></button>
<div 
    id="map" 
    class="map 
        {contentState.mapExpanded && !contentState.videoPlaying ? 'map_touchable' : ''}
        {featureOver ? 'map_feature-over' : ''}" 
    bind:this={mapContainer}
></div>

<style>
    .map {
        top: 0;
        left: 0;
        position: absolute;
        width: 100%;
        height: 100%;
        pointer-events: none;
    }

    .map_touchable {
        pointer-events: all;
    }

    :global(.map_feature-over canvas) {
        cursor: pointer !important;
    }

    .map__expand-bttn {
        top: 0;
        left: 0;
        position: absolute;
        width: 100%;
        height: 100%;
        pointer-events: none;
    }

    .map__expand-bttn_touchable {
        pointer-events: all;
    }
    
    :global(.maplibregl-control-container) {
        display: none;
    }
</style>