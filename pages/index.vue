<template>


  <main class="w-screen h-screen">
    <select class="round" id="layer-change">
    <option selected value="mapbox://styles/mapbox/streets-v11">Dark</option>
    <option value="mapbox://styles/mapbox/outdoors-v11">outdoors</option>
    <option value="mapbox://styles/mapbox/light-v10">light</option>
    <option value="mapbox://styles/mapbox/dark-v10">dark</option>
    <option value="mapbox://styles/mapbox/satellite-v9">satellite</option>
    <option value="mapbox://styles/mapbox/satellite-streets-v11">
      satellite-streets
    </option>
    <option value="mapbox://styles/mapbox/navigation-day-v1">
      navigation-day
    </option>
    <option value="mapbox://styles/mapbox/navigation-night-v1">
      navigation-night
    </option>
  </select>
    <v-map
      class="w-full h-full"
      :options="data.options"
      @loaded="onMapLoaded"
    />
    <!-- //@loaded="onMapLoaded" -->
  </main>
  <div id="map"></div>
</template>

//
// <script lang="ts">
// //   @import "~mapbox-gl/dist/mapbox-gl.css";
// //  @import "~v-mapbox/dist/v-mapbox.css";
// //  import VMap from "v-mapbox";
// //
// </script>

<script setup lang="ts">
import mapboxgl from "mapbox-gl";
import MapboxGeocoder from '@mapbox/mapbox-gl-geocoder';
import '@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css';
import VMap from "v-mapbox";

const data = reactive({
  options: {
    accessToken:
      "pk.eyJ1IjoiaG93YWxlLTIyIiwiYSI6ImNsNmdjeTVwMDBiYXEzam83dnRkNDgweW0ifQ.Zo9LsALFf3L0O4ptnZNQ1Q",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    center: [73.7997, 18.6298],
    zoom: 11,
    maxZoom: 22,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
    container:'map'
  } as mapboxgl.MapboxOptions,
    map: {} as mapboxgl.Map,
});

function onMapLoaded(map) {
  //   console.log('map loaded with ', map);

  //   map.loadImage('http://localhost:3000/assets/images/my-image.png', (error, image) => {
  //       if (error) throw error;
  //       if (!map.hasImage('myimage')) map.addImage('myimage', image);

  //       console.log('image: ', map.hasImage('myimage'));

  //    });
  // const marker = new mapboxgl.Marker({
  //   color: "red",
  //   draggable: true,
  // })
  //   .setLngLat([25.2048, 55.2708])
  //   .setPopup(
  //     new mapboxgl.Popup().setHTML("<b><i>Habi..bi... come to Dubai..</i></b>")
  //   )
  //   .addTo(map);

  // marker.togglePopup();

  // // fly to marked location
  // // map.flyTo({
  // //   center: [25.2048, 55.2708],
  // //   zoom: 9,
  // //   speed: 0.6,
  // //   curve: 1,
  // //   easing(t) {
  // //     return t;
  // //   },

  // // });
  
  
  // map.on("click", (e) => {
  //   console.log("clicked event occures at", e.lngLat.lat, e.lngLat.lng);
  //   console.log(e);
  //   new mapboxgl.Marker({
  //     draggable: true,
  //     color: randomcolor(),
  //   })
  //     .setLngLat([e.lngLat.lng, e.lngLat.lat])
  //     .addTo(map);
  // });

  // function randomcolor() {
  //   var latter = "0123456789ABCDEF";
  //   var color = "#";
  //   for (var i = 0; i < 6; i++) {
  //     color = color + latter[Math.floor(Math.random() * 16)];
  //   }
  //   return color;
  // }

  //  change map style with select option
  const setStyle: any = document.getElementById("layer-change");
  setStyle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle(event.target.value);
  });

  // map.addLayer({
  //   id: "points-of-interest",
  //   source: {
  //     type: "vector",
  //     url: "mapbox://mapbox.mapbox-streets-v8",
  //   },
  //   "source-layer": "poi_label",
  //   type: "circle",
  //   paint: {
  //     // Mapbox Style Specification paint properties
  //   },
  //   layout: {
  //     // Mapbox Style Specification layout properties
  //   },
  // });


  data.map = map;
    mapboxgl.accessToken = "pk.eyJ1IjoiaG93YWxlLTIyIiwiYSI6ImNsNmdjeTVwMDBiYXEzam83dnRkNDgweW0ifQ.Zo9LsALFf3L0O4ptnZNQ1Q";
    const geocoder = new MapboxGeocoder({
        accessToken: mapboxgl.accessToken,
        mapboxgl: mapboxgl
    });

    data.map.addControl(geocoder);
    // console.log('on map laoded: ', map);
    data.map.addSource('pune', {
        'type': 'geojson',
        "data": {
            'type': 'Feature',
            'geometry': {
                'type': 'Polygon',
                'coordinates': [
                    [
                        [
                            73.68942260742188,
                            18.530398219358684
                        ],
                        [
                            73.65509033203125,
                            18.340187242207897
                        ],
                        [
                            73.99154663085938,
                            18.359739156553683
                        ],
                        [
                            73.99429321289062,
                            18.641040231399984
                        ],
                        [
                            73.68942260742188,
                            18.530398219358684
                        ]
                    ]
                ]
            }
        }
    });
    // Add a new layer to visualize the polygon.
    data.map.addLayer({
        'id': 'pune',
        'type': 'line',
        'source': 'pune', // reference the data source
        'layout': {},
        
        'paint': {
            'line-color': 'black', // blue color fill
            'line-opacity': 0.5
        }
    });

}
</script>
<style>
html,
body {
  margin: 0;
  display: inline-block;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}
</style>
