<script setup lang="ts">
// Invitación de Casamiento
import MapLocation from '@/components/MapLocation.vue'
import { ref } from 'vue'

// Coordenadas de Quinta Pepe Reina - REEMPLAZAR CON LAS COORDENADAS REALES
const venueCoordinates = {
  latitude: -34.942449,  // Ejemplo: Buenos Aires
  longitude: -58.159328,
  locationName: 'Quinta Pepe Reina',
  address: 'Esq. Calle 430 Bis, Diagonal 434. Arturo Seguí'
}

// Alias para copiar al portapapeles
const bankAlias = 'La rompemos'
const copyButtonText = ref('Copiar alias')

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(bankAlias)
    copyButtonText.value = '¡Copiado!'
    
    // Volver al texto original después de 2 segundos
    setTimeout(() => {
      copyButtonText.value = 'Copiar alias'
    }, 2000)
  } catch (err) {
    console.error('Error al copiar:', err)
    copyButtonText.value = 'Error al copiar'
    setTimeout(() => {
      copyButtonText.value = 'Copiar alias'
    }, 2000)
  }
}
</script>

<template>
  <div class="wedding-invitation">
    <img src="@/assets/1_invitacion.svg" alt="Invitación de Casamiento" class="invitation-image">

    <img src="@/assets/2_info.svg" alt="Ubicacion y Fecha" class="invitation-image">

    <MapLocation 
      :latitude="venueCoordinates.latitude"
      :longitude="venueCoordinates.longitude"
      :location-name="venueCoordinates.locationName"
      :address="venueCoordinates.address"
    />

    <img src="@/assets/4_save_the_date.svg" alt="Guardar la Fecha" class="invitation-image">

    <a class="confirm-attendance-button" href="">Confirmar tu asistencia</a>

    <img src="@/assets/5_regalos_datos_bancarios.svg" alt="Link al Formulario de Google" class="invitation-image">
    
    <button @click="copyToClipboard" class="copy-alias-button">
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
        <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
      </svg>
      {{ copyButtonText }}
    </button>
  </div>
</template>

<style scoped>
.confirm-attendance-button {
  /* Mobile First - Base para todos los dispositivos */
  display: inline-block;
  margin: 2rem auto;
  padding: clamp(0.875rem, 3vw, 1.25rem) clamp(1.5rem, 5vw, 2.5rem);
  background-color: #632E70;
  color: #FFFFFF;
  text-decoration: none;
  font-size: clamp(1rem, 4vw, 1.35rem);
  font-weight: 600;
  border-radius: 50px; /* Bordes muy redondeados */
  transition: all 0.3s ease;
  text-align: center;
  box-shadow: 0 4px 12px rgba(99, 46, 112, 0.3);
  cursor: pointer;
  width: 90%;
  max-width: 400px;
  
  /* Mejorar legibilidad */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Efecto hover para dispositivos con mouse */
@media (hover: hover) {
  .confirm-attendance-button:hover {
    background-color: #7a3a87;
    box-shadow: 0 6px 16px rgba(99, 46, 112, 0.4);
    transform: translateY(-2px);
  }
}

/* Efecto activo (al presionar) */
.confirm-attendance-button:active {
  transform: translateY(0);
  box-shadow: 0 2px 8px rgba(99, 46, 112, 0.3);
}

/* Dispositivos muy pequeños (iPhone SE, Galaxy Fold cerrado) */
@media (max-width: 320px) {
  .confirm-attendance-button {
    padding: 0.75rem 1.25rem;
    font-size: 0.95rem;
    margin: 1.5rem auto;
    width: 95%;
  }
}

/* Móviles pequeños en vertical (321px - 374px) */
@media (min-width: 321px) and (max-width: 374px) {
  .confirm-attendance-button {
    padding: 0.875rem 1.5rem;
    font-size: 1rem;
    width: 92%;
  }
}

/* Móviles estándar en vertical (375px - 428px) */
@media (min-width: 375px) and (max-width: 428px) {
  .confirm-attendance-button {
    padding: 1rem 1.75rem;
    font-size: 1.15rem;
    width: 90%;
  }
}

/* Móviles grandes en vertical (429px - 480px) */
@media (min-width: 429px) and (max-width: 480px) {
  .confirm-attendance-button {
    padding: 1.125rem 2rem;
    font-size: 1.25rem;
  }
}

/* Móviles en horizontal / Tablets pequeñas (481px - 767px) */
@media (min-width: 481px) and (max-width: 767px) {
  .confirm-attendance-button {
    padding: 1.125rem 2.25rem;
    font-size: 1.25rem;
    width: 80%;
    max-width: 450px;
  }
}

/* Tablets en vertical (768px - 1024px) */
@media (min-width: 768px) and (max-width: 1024px) {
  .confirm-attendance-button {
    padding: 1.25rem 2.5rem;
    font-size: 1.35rem;
    width: 70%;
    max-width: 500px;
  }
}

/* Desktop (1025px+) */
@media (min-width: 1025px) {
  .confirm-attendance-button {
    padding: 1.25rem 2.5rem;
    font-size: 1.35rem;
    width: auto;
    max-width: 450px;
  }
}

/* Orientación horizontal en móviles */
@media (orientation: landscape) and (max-height: 500px) {
  .confirm-attendance-button {
    margin: 1rem auto;
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
  }
}

/* Safe area para dispositivos con notch */
@supports (padding: max(0px)) {
  .confirm-attendance-button {
    margin-bottom: max(2rem, env(safe-area-inset-bottom));
  }
}

/* Botón copiar alias - Estilo minimalista */
.copy-alias-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin: 1rem auto 2rem;
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
  width: auto;
  max-width: 300px;
}

.copy-alias-button svg {
  flex-shrink: 0;
}

/* Hover para dispositivos con mouse */
@media (hover: hover) {
  .copy-alias-button:hover {
    background-color: #632E70;
    color: #FFFFFF;
    transform: translateY(-1px);
  }
}

.copy-alias-button:active {
  transform: translateY(0);
  background-color: #7a3a87;
  color: #FFFFFF;
}

/* Dispositivos muy pequeños */
@media (max-width: 320px) {
  .copy-alias-button {
    padding: 0.625rem 1rem;
    font-size: 0.8rem;
    border-width: 1.5px;
    margin: 0.75rem auto 1.5rem;
  }
  
  .copy-alias-button svg {
    width: 16px;
    height: 16px;
  }
}

/* Safe area para dispositivos con notch */
@supports (padding: max(0px)) {
  .copy-alias-button {
    margin-bottom: max(2rem, env(safe-area-inset-bottom));
  }
}

.wedding-invitation {
  /* Mobile First Design - Base para todos los dispositivos */
  min-height: 100vh;
  min-height: 100dvh;
  width: 100%;
  background-color: #D4C1DB;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  overflow-x: hidden;
  
  /* Centrar contenido */
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
}

/* Estilos para todas las imágenes */
.invitation-image {
  width: 100%;
  height: auto; /* Altura automática según contenido */
  object-fit: contain;
  object-position: center;
  display: block;
  
  /* Padding responsive para que la imagen no toque los bordes */
  padding: clamp(0.5rem, 2vw, 1rem);
  box-sizing: border-box;
}

/* Solo la PRIMERA imagen ocupa toda la pantalla inicial */
.invitation-image:first-of-type {
  min-height: 100vh;
  min-height: 100dvh;
}

/* Dispositivos muy pequeños (iPhone SE, Galaxy Fold cerrado, etc.) */
@media (max-width: 320px) {
  .invitation-image {
    padding: 0.25rem;
  }
  
  .invitation-image:first-of-type {
    min-height: 100vh;
  }
}

/* Móviles pequeños en vertical (321px - 374px) */
@media (min-width: 321px) and (max-width: 374px) {
  .invitation-image {
    padding: 0.5rem;
  }
}

/* Móviles estándar en vertical (375px - 428px) - iPhone 12, 13, 14, Samsung Galaxy S21, etc. */
@media (min-width: 375px) and (max-width: 428px) {
  .invitation-image {
    padding: 0.75rem;
  }
}

/* Móviles grandes en vertical (429px - 480px) - iPhone Pro Max, Samsung Galaxy S21 Ultra, etc. */
@media (min-width: 429px) and (max-width: 480px) {
  .invitation-image {
    padding: 1rem;
  }
}

/* Móviles en horizontal / Tablets pequeñas en vertical (481px - 767px) */
@media (min-width: 481px) and (max-width: 767px) {
  .invitation-image {
    padding: 1rem;
    max-width: 600px;
  }
}

/* Tablets en vertical (768px - 1024px) - iPad, Samsung Tab, etc. */
@media (min-width: 768px) and (max-width: 1024px) {
  .invitation-image {
    padding: 1.5rem;
    max-width: 700px;
  }
}

/* Tablets grandes y Desktop pequeño (1025px+) */
@media (min-width: 1025px) {
  .invitation-image {
    padding: 2rem;
    max-width: 800px;
  }
}

/* Orientación horizontal específica para móviles */
@media (orientation: landscape) and (max-height: 500px) {
  .invitation-image {
    padding: 0.5rem 1rem;
  }
  
  .invitation-image:first-of-type {
    min-height: 100vh;
  }
}

/* Soporte para pantallas de alta densidad (Retina) */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .invitation-image {
    -webkit-font-smoothing: antialiased;
    image-rendering: -webkit-optimize-contrast;
    image-rendering: crisp-edges;
  }
}

/* Safe area para dispositivos con notch (iPhone X y superiores) */
@supports (padding: max(0px)) {
  .invitation-image {
    padding-left: max(0.5rem, env(safe-area-inset-left));
    padding-right: max(0.5rem, env(safe-area-inset-right));
    padding-top: max(0.5rem, env(safe-area-inset-top));
    padding-bottom: max(0.5rem, env(safe-area-inset-bottom));
  }
}
</style>
