<template>
  <!-- <button @click="title = 'Changed Popup Title'">Change Title</button> -->
  <div class="container">
    <div id="map" class="map" />
    <div class="sidebar">
      <PersonView v-if="people && index != -1" :name="people[index].nimi" :nick="people[index].hüüdnimi" :emoji="people[index].emoji" :value2="people[index].kodeerimine" :value1="people[index].kujundamine" :value3="people[index].asjaajamine" />
    </div>
  </div>
</template>

<script>

import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import { onMounted } from "vue";
// import { createApp, defineComponent, ref, nextTick } from "vue";
import { ref } from "vue";
import PersonView from "@/components/PersonView.vue";

export default {
  components: {
    PersonView
  },
  setup() {
    const title = ref("Unchanged Popup Title");
    const sheet = `https://sheets.googleapis.com/v4/spreadsheets/1OXlkK47osO7ZjOyQ3kqLg7_UZmPLG1GGTtLLJHk8Nhw/values/Sheet1?alt=json&majorDimension=ROWS&key=AIzaSyA5ofruKPLx4NuGvdi8OvE3aS6Vi8ADSME`
    let people = ref(null);
    let index = ref(-1);

    class ClickableMarker extends mapboxgl.Marker {
      onClick(handleClick) {
        this._handleClick = handleClick;
        return this;
      }
      _onMapClick(e) {
        const targetElement = e.originalEvent.target;
        const element = this._element;
        if (this._handleClick && (targetElement === element || element.contains((targetElement)))) {
          this._handleClick();
        }
      }
    }

    onMounted(() => {
      
      fetch(sheet)
        .then(res => res.json())
        .then(res => {
          people.value = toKeyedRows(res.values);
          people.value.forEach((dude, i) => {
            const el = document.createElement("div");
            el.innerText = dude.emoji;
            el.className = "marker";
            new ClickableMarker(el)
              .setLngLat(dude.koordinaadid.split(", ").reverse())
              .onClick(() => { 
                index.value = i;
              })
              .addTo(map);
          });
        });

      mapboxgl.accessToken = "pk.eyJ1Ijoib29rZXIiLCJhIjoiY2t0OGM4Nm1jMGo4ejJvcXFkZHFzMXdiMyJ9.Mf9gAf6lj40231xxaSjHIQ";
      const map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/streets-v11",
        bounds: [[21.368220124719926,59.774615762879044], [28.2142900936641,57.437860579312925]]
      });

      // map.on("load", () => {
        
      // });
    });


    const toKeyedRows = (data) => {
      const titles = data[0].map( (title) => { 
          return title.split(" ").join("").toLowerCase();
      });
      return data.slice(1).map( 
          row => Object.fromEntries(row.map((value, i) => ([titles[i], value])))
      );
    }

    return { title, index, people };
  },
};
</script>

<style>
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
body{
  margin:0;padding:0;
  font-family: sans-serif;
}
#map {
  height: 100vh;
}

.container {
  display: flex;
}
.map {
  flex: 3 1 75%;
}
.sidebar {
  flex: 1 0 25%;
}

.marker{
  font-size: 1.6em;
  cursor: pointer;
}
h1, h2{
  margin: 0;
}
.label{
  margin-bottom: 0.25rem;
}

</style>