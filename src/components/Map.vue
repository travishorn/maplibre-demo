<script setup>
import { onMounted } from "vue";
import maplibregl from "maplibre-gl";
import mapStyle from "../map-styles/maplibre.json";

const containerId = `map-${crypto.randomUUID()}`;

onMounted(() => {
  const map = new maplibregl.Map({
    container: containerId,
    style: mapStyle,
  });

  let hoveredId = null;

  map.on("mousemove", "countries-fill", function (e) {
    if (e.features.length > 0) {
      if (hoveredId) {
        map.setFeatureState(
          { source: "maplibre", id: hoveredId, sourceLayer: "countries" },
          { hover: false }
        );
      }

      hoveredId = e.features[0].id;

      map.setFeatureState(
        { source: "maplibre", id: hoveredId, sourceLayer: "countries" },
        { hover: true }
      );
    }
  });

  map.on("mouseleave", "countries-fill", function () {
    if (hoveredId) {
      map.setFeatureState(
        { source: "maplibre", id: hoveredId, sourceLayer: "countries" },
        { hover: false }
      );
    }
    hoveredId = null;
  });

  map.on("mouseup", "countries-fill", function (e) {
    console.log(e.features[0].properties);
  });

  map.on("load", () => {
    console.log(map.getSource("maplibre"));
  });
});
</script>

<template>
  <div class="map" :id="containerId"></div>
</template>
