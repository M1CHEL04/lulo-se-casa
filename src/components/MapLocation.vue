<script setup lang="ts">
import { onMounted, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

// Props para recibir coordenadas
const props = defineProps<{
  latitude: number
  longitude: number
  locationName: string
  address: string
}>()

const mapContainer = ref<HTMLElement | null>(null)

onMounted(() => {
  if (!mapContainer.value) return

  // Crear el mapa centrado en las coordenadas
  const map = L.map(mapContainer.value, {
    center: [props.latitude, props.longitude],
    zoom: 16,
    scrollWheelZoom: false, // Desactivar zoom con scroll para móviles
    dragging: true,
    zoomControl: true
  })

  // Agregar tiles de CartoDB Positron (estilo minimalista y elegante)
  L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
    attribution: '© OpenStreetMap contributors © CARTO',
    maxZoom: 19,
    subdomains: 'abcd'
  }).addTo(map)

  // Crear ícono personalizado para el pin
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

  // Agregar marcador en la ubicación
  L.marker([props.latitude, props.longitude], { icon: customIcon })
    .addTo(map)
    .bindPopup(`<strong>${props.locationName}</strong><br>${props.address}`)
})

// Funciones para abrir en apps externas
const openInGoogleMaps = () => {
  // Place ID de Quinta Pepe Reina
  const placeId = 'ChIJr2ZlDKbbopURauFuyvDyLVQ'
  
  // Para móviles: intenta abrir la app nativa de Google Maps
  // Para desktop: abre en el navegador
  const url = `https://www.google.com/maps/search/?api=1&query=${props.latitude},${props.longitude}&query_place_id=${placeId}`
  
  window.open(url, '_blank')
}

const openInWaze = () => {
  // URL específica de Quinta Pepe Reina en Waze
  const url = 'https://www.waze.com/ul?ll=-34.9423244,-58.1586701&navigate=yes&zoom=17'
  
  window.open(url, '_blank')
}
</script>

<template>
  <div class="map-section">
    <div class="map-container" ref="mapContainer"></div>
    
    <div class="navigation-buttons">
      <button @click="openInGoogleMaps" class="nav-button google-maps">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
          <path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z"/>
        </svg>
        Abrir en Google Maps
      </button>
      
      <button @click="openInWaze" class="nav-button waze">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
          <path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z"/>
        </svg>
        Abrir en Waze
      </button>
    </div>
  </div>
</template>

<style scoped>
.map-section {
  width: 100%;
  min-height: auto; /* Altura automática según contenido */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: clamp(0.5rem, 2vw, 1rem);
  box-sizing: border-box;
  gap: 1.5rem;
}

.map-container {
  width: 100%;
  max-width: 800px;
  height: clamp(300px, 50vh, 500px);
  border: 4px solid #632E70;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 24px rgba(99, 46, 112, 0.3);
  background-color: #D4C1DB;
  position: relative;
}

/* Filtro de color para integrar el mapa con la paleta */
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
  max-width: 800px;
}

.nav-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: clamp(0.75rem, 2.5vw, 0.875rem) clamp(1.25rem, 4vw, 1.75rem);
  background-color: transparent;
  color: #632E70;
  border: 2px solid #632E70;
  border-radius: 50px;
  font-size: clamp(0.875rem, 3vw, 1rem);
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.nav-button svg {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

/* Hover para dispositivos con mouse */
@media (hover: hover) {
  .nav-button:hover {
    background-color: #632E70;
    color: #FFFFFF;
    transform: translateY(-1px);
  }
}

.nav-button:active {
  transform: translateY(0);
  background-color: #7a3a87;
  color: #FFFFFF;
}

/* Dispositivos muy pequeños */
@media (max-width: 320px) {
  .map-container {
    height: 250px;
    border-width: 3px;
  }
  
  .navigation-buttons {
    gap: 0.75rem;
  }
  
  .nav-button {
    padding: 0.625rem 1rem;
    font-size: 0.8rem;
    border-width: 1.5px;
  }
  
  .nav-button svg {
    width: 16px;
    height: 16px;
  }
}

/* Móviles en horizontal */
@media (min-width: 481px) {
  .navigation-buttons {
    flex-direction: row;
  }
  
  .nav-button {
    flex: 1;
  }
}

/* Tablets y Desktop */
@media (min-width: 768px) {
  .map-container {
    height: 450px;
  }
}

/* Safe area para dispositivos con notch */
@supports (padding: max(0px)) {
  .map-section {
    padding-left: max(0.5rem, env(safe-area-inset-left));
    padding-right: max(0.5rem, env(safe-area-inset-right));
  }
}

/* Estilos globales para Leaflet (sin scoped) */
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
