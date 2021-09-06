<template>
  <!-- <button @click="title = 'Changed Popup Title'">Change Title</button> -->
  <div class="container">
    <div id="map" class="map" />
    <div class="info-panel">
      <template v-if="people">
        <h1>Nimi: {{ people[index].nimi }}</h1>
        <h2>Hüüdnimi: {{ people[index].hüüdnimi }}</h2>
      </template>
    </div>
  </div>
</template>

<script>

import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import { onMounted } from "vue";
// import { createApp, defineComponent, ref, nextTick } from "vue";
import { ref } from "vue";
// import MyPopup from "@/components/MyPopup.vue";

export default {
  setup() {
    const title = ref("Unchanged Popup Title");
    const sheet = `https://sheets.googleapis.com/v4/spreadsheets/1OXlkK47osO7ZjOyQ3kqLg7_UZmPLG1GGTtLLJHk8Nhw/values/Sheet1?alt=json&majorDimension=ROWS&key=AIzaSyA5ofruKPLx4NuGvdi8OvE3aS6Vi8ADSME`
    let people = ref(null);

    let index = ref(-1);
    // let loaded = false;

    // extend mapboxGL Marker so we can pass in an onClick handler
    class ClickableMarker extends mapboxgl.Marker {
      // new method onClick, sets _handleClick to a function you pass in
      onClick(handleClick) {
        this._handleClick = handleClick;
        return this;
      }
      // the existing _onMapClick was there to trigger a popup
      // but we are hijacking it to run a function we define
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
          console.log(people.value);


          people.value.forEach((dude, i) => {
            const el = document.createElement("div");
            el.innerText = dude.emoji;
            el.className = "marker";
            new ClickableMarker(el)
              .setLngLat([dude.long, dude.lat])
              .onClick(() => { 
                index.value = i;
              })
              .addTo(map);
          });

        //   new ClickableMarker()
        // .setLngLat([24.71454576451196, 58.6350362443377])
        // .onClick(() => { // onClick() is a thing now!
        //   // document.getElementById('info-box')
        //   //   .innerHTML = `You clicked ${city.name}!`;
        //   console.log('KLIKK, BABY!');
        // })
        // .addTo(map);

        });
      
      
      
      mapboxgl.accessToken = "pk.eyJ1Ijoib29rZXIiLCJhIjoiY2t0OGM4Nm1jMGo4ejJvcXFkZHFzMXdiMyJ9.Mf9gAf6lj40231xxaSjHIQ";
      const map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/light-v9",
        zoom: 7,
        center: [24.71454576451196, 58.6350362443377]
      });

      map.on("load", () => {
        


        


        // // Here we want to setup the dropdown
        // map.on("click", "usa-fill", function (e) {
        //   new mapboxgl.Popup()
        //     .setLngLat(e.lngLat)
        //     .setHTML('<div id="popup-content"></div>')
        //     .addTo(map);
        //   const MyNewPopup = defineComponent({
        //     extends: MyPopup,
        //     setup() {
        //       return { title };
        //     },
        //   });
        //   nextTick(() => {
        //     createApp(MyNewPopup).mount("#popup-content");
        //   });
        // });
      });
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
body{
  margin:0;padding:0;
}
#map {
  height: 100vh;
}
.container {
  display: flex;
}
.map {
  flex: 3 3 75%;
}
.info-panel {
  flex: 1 1 25%;
  padding: 3vh;
}
.marker{
  font-size: 1.6em;
}
</style>