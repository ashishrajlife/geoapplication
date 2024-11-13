<template>
    <div class="page-wrapper">
      <header class="header">
        <Header />
      </header>
      <div class="content">
        <div class="map-container">
          <div v-if="view" id="map">
            <mapView/></div>
          <div v-else id="map">No data to show....</div>
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
export default {
    name: 'homeView',
    components:{
        Header,
        Footer,
        mapView
    },
    data() {
        return {
            cars:[],
            view:true
        }
    },
}
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

#map {
  width: 100%;
  height: 100%;
  background-color: #e0e0e0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  color: #888;
}
</style>


