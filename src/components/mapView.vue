<template>
    <link rel="stylesheet" href="https://api.mapbox.com/mapbox-gl-js/v2.13.0/mapbox-gl.css" />
    <div id="map">
    </div>
    <div class="mt-4 d-flex">   <button class="measure-btn" @click="enableMeasure">Measure Distance</button></div>
  
    <div v-if="showpopup" class="custom-popup">
    <div class="popup-inner">
     <div> <h5>Distance Measurement</h5> <span class="close-button"> <img src="../../src/assets/images/remove.png" alt="Close" class="close-icon" @click="closePopup" /></span> </div>
     <div class="line"></div>
     <p class="distance-value">Kilometers: <span>{{ kilometer }} km</span></p>
      <p class="distance-value">Miles: <span>{{ miles }} mi</span></p>
    </div>
  </div>
  </template>
  
  <script>
  import axios from "axios";
  import mapboxgl from "mapbox-gl";
  import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
  
  export default {
    data() {
      return {
        map: null,
        geoData: null,
        markers: [],
        showpopup:false,
        kilometer:'',
        miles:''
      };
    },
    mounted() {
      mapboxgl.accessToken = "pk.eyJ1IjoiYXNoaXNocmFqbGlmZSIsImEiOiJjbTJtZDN6amgwanp6MmlyMnlkbThoNXpoIn0.sv_-eIgiFlVfJYsmWHWImQ";
      this.initializeMap();
      this.fetchGeoData();
    },
    methods: {
      initializeMap() {
        this.map = new mapboxgl.Map({
          container: "map",
          style: "mapbox://styles/mapbox/streets-v11",
          center: [78.9629, 20.5937],
          zoom: 4,
        });
  
        this.map.addControl(new mapboxgl.NavigationControl());
      },
      async fetchGeoData() {
        try {
          const response = await axios.get("http://localhost:3000/geodata");
          const geoData = response.data[0].geoData;
  
          if (!geoData || geoData.type !== "FeatureCollection" || !geoData.features) {
            console.error("Invalid GeoJSON data structure:", response.data);
            alert("Invalid GeoJSON data received from server.");
            return;
          }
  
          this.geoData = geoData;
          this.addMarkers();
  
        } catch (error) {
          console.error("Error fetching GeoJSON data:", error);
        }
      },
      addMarkers() {
        if (!this.geoData || !this.geoData.features) {
          console.error("No valid GeoJSON data for markers.");
          return;
        }
  
        this.geoData.features.forEach((feature) => {
          const coordinates = feature.geometry.coordinates;
          const properties = feature.properties;
  
          const marker = new mapboxgl.Marker({ draggable: true })
            .setLngLat(coordinates)
            .addTo(this.map);
  
          const popup = new mapboxgl.Popup({ offset: 25 }).setHTML(
            `<h6>${properties.name}</h6> <span>${properties.description}</span>`
          );
  
          marker.setPopup(popup);
  
          marker.getElement().addEventListener('mouseenter', () => {
            popup.addTo(this.map);
          });
          marker.getElement().addEventListener('mouseleave', () => {
            popup.remove();
          });
  
          marker.on('dragend', () => {
            const newCoordinates = marker.getLngLat();
            console.log("Marker dragged to:", newCoordinates);
            feature.geometry.coordinates = [newCoordinates.lng, newCoordinates.lat];
          });
        });
      },
      enableMeasure() {
        this.markers = []; 
        toast.warn('Click Two locations one by one !',{ autoClose:10000 });
        this.map.on("click", this.addMeasureMarker);
      },
      addMeasureMarker(event) {
        if (this.markers.length < 2) {
          const markerElement = document.createElement('div');
          
          const marker = new mapboxgl.Marker(markerElement)
            .setLngLat(event.lngLat)
            .addTo(this.map);
  
          this.markers.push(marker);
          if (this.markers.length === 2) {
            this.calculateDistance();
          }
        }
      },
      calculateDistance() {
        if (this.markers.length === 2) {
          const marker1 = this.markers[0].getLngLat();
          const marker2 = this.markers[1].getLngLat();
  
          const distanceInMeters = marker1.distanceTo(marker2);

          const distanceInKm = (distanceInMeters / 1000).toFixed(2);
          const distanceInMiles = (distanceInMeters / 1609.34).toFixed(2);
 
         this.showpopup=true;
         this.kilometer = distanceInKm;
         this.miles = distanceInMiles;
         console.log('kmmiles -----' , this.kilometer,this.miles,this.showpopup)

          this.markers.forEach(marker => marker.remove());
          this.markers = [];
          this.map.off("click", this.addMeasureMarker);
        }
      },
      closePopup(){
        this.showpopup = false;
      }
    },
  };
  </script>
  
  <style scoped>
  #map {
    width: 100%;
    height: 400px;
  }
  button {
    margin-top: 10px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
  .line {
  height: 2px; 
  background-color: white; 
  margin: 10px 0; 
  width: 100%;
}
  .custom-marker {
 border: 5px solid black;
 z-index: 9999999;
  }
  .measure-btn {
    position: absolute;
    right: 80%;
    bottom: 40px;
    padding: 10px 40px;
    top: 81%;
    font-size: 16px;
    background-color: black;
    color: yellow;
    border: 2px solid;
    border-radius: 50px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.8);
    cursor: pointer;
    z-index: 1000;
}
.custom-popup {
  position: fixed;
  top: 30%;
  left: 50%;
  transform: translateX(-50%);
  background: black;
  color: white;
  padding: 20px;
  border-radius: 10px;
  font-size: 16px;
  z-index: 1001;
}

.popup-inner {
  text-align: center;
  margin-top: 25px;
}

.distance-value span {
  color: yellow;
  font-weight: bold;
}

.close-icon {
  width: 25px;
    height: 25px;
    position: absolute;
    cursor: pointer;
    top: 10px;
    right: 10px;
}
</style>