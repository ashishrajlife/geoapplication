<template>
  <div class="page-wrapper">
    <header class="header">
      <Header />
    </header>
    <div class="content">
      <div class="map-container">
        <input  type="file" class="add-btn" @change="handleFileUpload" accept=".geojson,application/geo+json" ref="fileInput" />

        <span class="add-btn clear-btn" @click="clearData">Clear</span>

        <div v-if="view" id="map">
          <mapView :geoJsonData="location" />
        </div>
        <div v-else id="map">
         Please  Upload File !
        </div>
      </div>
    </div>
    <footer class="footer">
      <Footer />
    </footer>
  </div>
</template>

<script>
import Header from './Header.vue';
import Footer from './Footer.vue';
import mapView from './mapView.vue';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
import axios from 'axios';

export default {
  name: 'homeView',
  components: {
    Header,
    Footer,
    mapView
  },
  data() {
    return {
      location: null,
      view: false,
      uploadedDataId: null
    };
  },
  methods: {
    async handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        this.view = true;
        const reader = new FileReader();
        reader.onload = async (e) => {
          try {
            const geoJsonData = JSON.parse(e.target.result);
            this.location = geoJsonData;

            const payload = { geoData: geoJsonData };
            try {
              const response = await axios.post('http://localhost:3000/geodata', payload);
              console.log('saved bro');
              toast.success('Uploaded Successfully',{ autoClose:3500 });
              this.uploadedDataId = response.data.id;
            } catch (error) {
              console.error('Error saving data ---> ', error);
            }
          } catch (error) {
            console.error('Invalid-->', error);
            this.location = null;
            this.view = false;
            this.$refs.fileInput.value = null;
            toast.error('Please upload a valid file !',{ autoClose:2500 });
          }
        };
        reader.readAsText(file);
      }
    },

    async clearData() {
      if (this.uploadedDataId) {
        try {
          await axios.delete(`http://localhost:3000/geodata/${this.uploadedDataId}`);
          this.uploadedDataId = null;
          toast.error('Deleted Successfully !',{
        autoClose:3500
      });
        } catch (error) {
          console.error('Error deleting data from server:', error);
        }
      }

      this.location = null;
      this.view = false;
      this.$refs.fileInput.value = null;
    }
  }
};
</script>


<style scoped>
.page-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  font-family: Arial, sans-serif;
}

.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  background: black;
}

.map-container {
  width: 100%;
  height: 500px;
  background-color: #f1f1f1;
  border-radius: 8px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  position: relative;
}

.add-btn {
  position: absolute;
  bottom: 40px;
  right: 40px;
  padding: 15px 40px;
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
.clear-btn{
  background-color: black;
  color: yellow;
  border: 3px solid white;
}

.add-btn:hover {
  background-color: rgb(87, 87, 59);
}

#map {
  width: 100%;
  height: 100%;
  background-color: #5e5e5e;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.7rem;
  color: rgb(0, 0, 0);
}
</style>
