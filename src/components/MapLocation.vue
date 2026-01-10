<script setup lang="ts">
import { onMounted, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const props = defineProps<{
  latitude: number
  longitude: number
  locationName: string
  address: string
}>()

const mapContainer = ref<HTMLElement | null>(null)

onMounted(() => {
  if (!mapContainer.value) return

  const map = L.map(mapContainer.value, {
    center: [props.latitude, props.longitude],
    zoom: 16,
    scrollWheelZoom: false,
    dragging: false,
    doubleClickZoom: false,
    boxZoom: false,
    keyboard: false,
    touchZoom: false,
    zoomControl: false
  })

  L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
    attribution: '© OpenStreetMap contributors © CARTO',
    maxZoom: 19,
    subdomains: 'abcd'
  }).addTo(map)

  const customIcon = L.divIcon({
    className: 'custom-marker',
    html: `
      <div style="
        width: 40px;
        height: 40px;
        background-color: #632E70;
        border: 3px solid white;
        border-radius: 50% 50% 50% 0;
        transform: rotate(-45deg);
        box-shadow: 0 3px 8px rgba(0,0,0,0.3);
      ">
        <div style="
          width: 12px;
          height: 12px;
          background-color: white;
          border-radius: 50%;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
        "></div>
      </div>
    `,
    iconSize: [40, 40],
    iconAnchor: [20, 40]
  })

  L.marker([props.latitude, props.longitude], { icon: customIcon })
    .addTo(map)
    .bindPopup(`<strong>${props.locationName}</strong><br>${props.address}`)
})

const openInGoogleMaps = () => {
  const placeId = 'ChIJr2ZlDKbbopURauFuyvDyLVQ'
  const url = `https://www.google.com/maps/search/?api=1&query=${props.latitude},${props.longitude}&query_place_id=${placeId}`
  window.open(url, '_blank')
}

const openInWaze = () => {
  const url = 'https://waze.com/ul/h69y9hwfnh'
  window.open(url, '_blank')
}
</script>

<template>
  <div class="map-section">
    <div class="map-container" ref="mapContainer"></div>
    
    <div class="navigation-buttons">
      <button @click="openInGoogleMaps" class="nav-button google-maps">
        <img src="https://cdn.simpleicons.org/googlemaps/D4C1DB" width="20" height="20" alt="" aria-hidden="true" />
        Abrir en Google Maps
      </button>
      
      <button @click="openInWaze" class="nav-button waze">
        <img src="https://cdn.simpleicons.org/waze/D4C1DB" width="20" height="20" alt="" aria-hidden="true" />
        Abrir en Waze
      </button>
    </div>
  </div>
</template>

<style scoped>
.map-section {
  width: 100%;
  min-height: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0; /* Sin padding adicional, lo maneja la secci\u00f3n padre */
  box-sizing: border-box;
  gap: 1.5rem;
}

/* Overrides para respetar el mismo margen lateral que las imágenes */
.map-section { padding: 0 3rem; }

@media (max-width: 320px) {
  .map-section { padding: 0 2rem; }
}

@media (min-width: 321px) and (max-width: 374px) {
  .map-section { padding: 0 2.25rem; }
}

@media (min-width: 375px) and (max-width: 428px) {
  .map-section { padding: 0 2.5rem; }
}

@media (min-width: 429px) and (max-width: 480px) {
  .map-section { padding: 0 2.75rem; }
}

@media (min-width: 481px) and (max-width: 767px) {
  .map-section { padding: 0 3rem; }
}

@media (min-width: 768px) and (max-width: 1024px) {
  .map-section { padding: 0 3.5rem; }
}

@media (min-width: 1025px) {
  .map-section { padding: 0 4rem; }
}

@media (orientation: landscape) and (max-height: 500px) {
  .map-section { padding: 0 2.5rem; }
}

@supports (padding: max(0px)) {
  .map-section {
    padding-left: max(2.5rem, env(safe-area-inset-left));
    padding-right: max(2.5rem, env(safe-area-inset-right));
  }
}
.map-container {
  width: 100%;
  max-width: 500px; /* Mismo ancho que las imágenes */
  height: clamp(250px, 40vh, 400px);
  border: 4px solid #632E70;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 24px rgba(99, 46, 112, 0.3);
  background-color: #D4C1DB;
  position: relative;
}

.map-container::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(to bottom, rgba(212, 193, 219, 0.15), rgba(99, 46, 112, 0.08));
  pointer-events: none;
  z-index: 400;
  mix-blend-mode: multiply;
}

.navigation-buttons {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 100%;
  max-width: 500px; /* Consistente con el ancho de las imágenes */
}

.nav-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  padding: clamp(0.875rem, 3vw, 1.25rem) clamp(1.5rem, 5vw, 2.5rem);
  background-color: #632E70;
  color: #D4C1DB;
  border: none;
  border-radius: 50px;
  font-size: clamp(1rem, 4vw, 1.35rem);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(99, 46, 112, 0.3);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
}

.nav-button svg {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

.nav-button img {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

@media (hover: hover) {
  .nav-button:hover {
    background-color: #7a3a87;
    box-shadow: 0 6px 16px rgba(99, 46, 112, 0.4);
    transform: translateY(-2px);
  }
}

.nav-button:active {
  transform: translateY(0);
  box-shadow: 0 2px 8px rgba(99, 46, 112, 0.3);
}

@media (max-width: 320px) {
  .map-section {
    gap: 1rem;
  }
  
  .map-container {
    height: 200px;
    border-width: 3px;
  }
  
  .navigation-buttons {
    gap: 0.75rem;
  }
  
  .nav-button {
    padding: 0.75rem 1.25rem;
    font-size: 0.95rem;
  }
}

@media (min-width: 321px) and (max-width: 374px) {
  .map-container {
    height: 220px;
  }
}

@media (min-width: 375px) and (max-width: 428px) {
  .map-container {
    height: 250px;
  }
}

@media (min-width: 429px) and (max-width: 480px) {
  .map-container {
    height: 280px;
  }
}

@media (min-width: 481px) and (max-width: 767px) {
  .map-container {
    height: 300px;
  }
  
  .navigation-buttons {
    flex-direction: row;
  }
  
  .nav-button {
    flex: 1;
  }
}

@media (min-width: 768px) and (max-width: 1024px) {
  .map-container {
    height: 350px;
  }
}

@media (min-width: 1025px) {
  .map-container {
    height: 400px;
  }
}

@supports (padding: max(0px)) {
  .map-section {
    padding-left: max(0rem, env(safe-area-inset-left));
    padding-right: max(0rem, env(safe-area-inset-right));
  }
}

:deep(.leaflet-popup-content-wrapper) {
  background-color: #632E70;
  color: white;
  border-radius: 8px;
}

:deep(.leaflet-popup-tip) {
  background-color: #632E70;
}

:deep(.custom-marker) {
  background: transparent;
  border: none;
}
</style>