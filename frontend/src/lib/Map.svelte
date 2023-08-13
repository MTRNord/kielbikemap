<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import maplibre from "maplibre-gl";
  import "maplibre-gl/dist/maplibre-gl.css";
  import mapStyle from "./mapStyle";
  import * as pmtiles from "pmtiles";
  // Add pmtiles protocol handling
  const protocol = new pmtiles.Protocol();
  maplibre.addProtocol("pmtiles", protocol.tile);

  let map;
  let mapContainer;

  onMount(() => {
    map = new maplibre.Map({
      container: mapContainer,
      style: mapStyle,
      center: [10.24441, 54.38356], // starting position [lng, lat]
      zoom: 9.15, // starting zoom
      maxBounds: [
        [7.82, 53.34],
        [11.32, 55.08],
      ], // based on coords in bbox.poly
      maxZoom: 20,
      // customAttribution:
      //   "&copy; SydneyBikeMap &copy; <a href='https://www.openstreetmap.org/copyright'>OpenStreetMap</a> contributors",
      hash: "map",
    });

    // Add controls
    map.addControl(new maplibre.NavigationControl({}));
    map.addControl(new maplibre.ScaleControl({}));
    map.addControl(
      new maplibre.GeolocateControl({
        positionOptions: {
          enableHighAccuracy: true,
        },
        trackUserLocation: true,
      })
    );
  });

  onDestroy(() => {
    map.remove();
  });
</script>

<div class="map" id="map" bind:this={mapContainer} />

<style>
  .map {
    position: relative;
    flex-grow: 1;
  }
</style>
