<script setup lang="ts">
// Invitación de Casamiento
import MapLocation from '@/components/MapLocation.vue'
import { ref, onMounted, onBeforeUnmount } from 'vue'

// Coordenadas de Quinta Pepe Reina
const venueCoordinates = {
  latitude: -34.942449,
  longitude: -58.159328,
  locationName: 'Quinta Pepe Reina',
  address: 'Esq. Calle 430 Bis, Diagonal 434. Arturo Seguí'
}

// Alias para copiar al portapapeles
const bankAlias = 'bodaluloyxime'
const copyButtonText = ref('Copiar alias')

const showScrollArrow = ref(true)

const handleScroll = () => {
  const viewportHeight = window.innerHeight
  const pageHeight = document.documentElement.scrollHeight

  // Si no hay suficiente contenido para scrollear, no mostramos la flecha
  if (pageHeight <= viewportHeight + 16) {
    showScrollArrow.value = false
    return
  }

  const scrollY = window.scrollY || window.pageYOffset

  // Mostramos la flecha solo cuando el usuario está cerca de la parte
  // superior (primer "pantallazo" / primera sección). Si baja más que
  // este umbral, se oculta, y vuelve a aparecer si vuelve a subir.
  showScrollArrow.value = scrollY <= viewportHeight * 0.15
}

onMounted(() => {
  handleScroll()
  window.addEventListener('scroll', handleScroll, { passive: true })
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll)
})

const scrollDown = () => {
  const nextSection = document.querySelector('.wedding-invitation .section:nth-of-type(2)') as HTMLElement | null

  if (nextSection) {
    const rect = nextSection.getBoundingClientRect()
    const target = window.scrollY + rect.top - 24
    window.scrollTo({ top: target, behavior: 'smooth' })
  } else {
    window.scrollBy({ top: window.innerHeight * 0.8, behavior: 'smooth' })
  }
}

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(bankAlias)
    copyButtonText.value = '¡Copiado!'
    
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
    <!-- Sección 1: Invitación -->
    <div class="section">
      <img src="@/assets/1_invitacion.svg" alt="Invitación de Casamiento" class="invitation-image">
    </div>

    <!-- Sección 2: Info + Mapa -->
    <div class="section">
      <img src="@/assets/2_info.svg" alt="Ubicacion y Fecha" class="invitation-image">
      <MapLocation 
        :latitude="venueCoordinates.latitude"
        :longitude="venueCoordinates.longitude"
        :location-name="venueCoordinates.locationName"
        :address="venueCoordinates.address"
      />
    </div>
    
    <!-- Sección 3: Dresscode -->
    <div class="section">
      <img src="@/assets/3_dresscode.svg" alt="Drescode" class="invitation-image">
    </div>
    
    <!-- Sección 4: Regalos -->
    <div class="section">
      <img src="@/assets/5_regalos.svg" alt="Colaboracion" class="invitation-image">
    </div>

    <!-- Sección 5: Datos bancarios + Botón copiar -->
    <div class="section">
      <img src="@/assets/6_datos_bancarios.svg" alt="Datos Bancarios" class="invitation-image">
      <button @click="copyToClipboard" class="copy-alias-button">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
          <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
        </svg>
        {{ copyButtonText }}
      </button>
    </div>

    <!-- Sección 6: Guardar la fecha + Confirmar asistencia -->
    <div class="section">
      <img src="@/assets/4_save_the_date.svg" alt="Guardar la Fecha" class="invitation-image">
      <a class="confirm-attendance-button" href="https://forms.gle/5dRSbUNw1rykz6g2A">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
          <polyline points="14 2 14 8 20 8"/>
          <path d="M9 15l2 2 4-4"/>
        </svg>
        Confirmar tu asistencia
      </a>
    </div>

    <div
      v-if="showScrollArrow"
      class="scroll-indicator"
    >
      <span class="scroll-indicator__text">Deslizá para ver más</span>
      <div class="scroll-indicator__icon">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M12 4v14" />
          <polyline points="6 14 12 20 18 14" />
        </svg>
      </div>
    </div>
  </div>
</template>

<style scoped>
.confirm-attendance-button,
.copy-alias-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  padding: clamp(0.875rem, 3vw, 1.25rem) clamp(1.5rem, 5vw, 2.5rem);
  background-color: #632E70;
  color: #D4C1DB;
  text-decoration: none;
  font-size: clamp(1rem, 4vw, 1.35rem);
  font-weight: 600;
  line-height: 1;
  border-radius: 50px;
  border: none;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(99, 46, 112, 0.3);
  cursor: pointer;
  width: 100%;
  max-width: 500px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.confirm-attendance-button {
  margin: 0 auto 4rem; /* Un poco más de espacio debajo */
}

.copy-alias-button {
  margin: 0 auto;
}

.confirm-attendance-button svg,
.copy-alias-button svg {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

@media (hover: hover) {
  .confirm-attendance-button:hover {
    background-color: #7a3a87;
    box-shadow: 0 6px 16px rgba(99, 46, 112, 0.4);
    transform: translateY(-2px);
  }
}

.confirm-attendance-button:active {
  transform: translateY(0);
  box-shadow: 0 2px 8px rgba(99, 46, 112, 0.3);
}

@media (max-width: 320px) {
  .confirm-attendance-button {
    padding: 0.75rem 1.25rem;
    font-size: 0.95rem;
  }
  .confirm-attendance-button svg,
  .copy-alias-button svg {
    width: 18px;
    height: 18px;
  }
}

@media (orientation: landscape) and (max-height: 500px) {
  .confirm-attendance-button {
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
  }
}
@media (hover: hover) {
  .copy-alias-button:hover {
    background-color: #7a3a87;
    box-shadow: 0 6px 16px rgba(99, 46, 112, 0.4);
    transform: translateY(-2px);
  }
}

.copy-alias-button:active {
  transform: translateY(0);
  box-shadow: 0 2px 8px rgba(99, 46, 112, 0.3);
}

@media (max-width: 320px) {
  .copy-alias-button {
    padding: 0.75rem 1.25rem;
    font-size: 0.95rem;
  }
}

.wedding-invitation {
  min-height: 100vh;
  min-height: 100dvh;
  width: 100%;
  background-color: #D4C1DB;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4rem; /* ESPACIO FIJO Y CONSISTENTE ENTRE SECCIONES */
}

.scroll-indicator {
  position: fixed;
  left: 50%;
  bottom: clamp(1rem, 3vh, 2rem);
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
  cursor: default;
  z-index: 20;
  user-select: none;
  text-align: center;
}

.scroll-indicator__text {
  font-size: 0.75rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  font-weight: 500;
  color: #7a3a87;
}

.scroll-indicator__icon {
  width: 46px;
  height: 46px;
  border-radius: 999px;
  border: 2px solid rgba(99, 46, 112, 0.55);
  background: radial-gradient(circle at 30% 0%, rgba(255, 255, 255, 0.9), rgba(212, 193, 219, 0.4));
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 9px 20px rgba(99, 46, 112, 0.35);
  color: #632E70;
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
  animation: scroll-float 2.2s ease-in-out infinite;
}

.scroll-indicator__icon svg {
  width: 22px;
  height: 22px;
  stroke: currentColor;
  fill: none;
  stroke-width: 1.7;
  stroke-linecap: round;
  stroke-linejoin: round;
  animation: scroll-arrow 1.4s ease-in-out infinite;
}

@media (hover: hover) {
  .scroll-indicator:hover .scroll-indicator__icon {
    transform: translateY(-2px);
    box-shadow: 0 12px 26px rgba(99, 46, 112, 0.42);
    border-color: rgba(99, 46, 112, 0.9);
  }
}

.scroll-indicator:active .scroll-indicator__icon {
  transform: translateY(0);
  box-shadow: 0 5px 14px rgba(99, 46, 112, 0.35);
}

@keyframes scroll-float {
  0%, 100% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, 6px, 0);
  }
}

@keyframes scroll-arrow {
  0%, 100% {
    transform: translateY(0);
    opacity: 0.8;
  }
  50% {
    transform: translateY(3px);
    opacity: 1;
  }
}

.section {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  padding: 0 3rem; /* Margen lateral homogéneo para todas las secciones */
  box-sizing: border-box;
}

.section:not(:first-child) {
  margin-top: clamp(1.5rem, 4vw, 2.75rem);
} 

.section:first-child {
  gap: 0;
  margin-bottom: -4rem; /* Compensa el gap de 4rem del contenedor para que el espacio abajo sea igual al de arriba */
  min-height: 100vh; /* Fallback */
  min-height: 100dvh; /* Ocupa el alto dinámico de la pantalla para mostrar solo la primera imagen al ingresar, incluso con barras del navegador */
  justify-content: center; /* Centra verticalmente la imagen principal */
  padding-top: clamp(1rem, 6vh, 3rem); /* margen superior interno */
  padding-bottom: clamp(1rem, 6vh, 3rem); /* margen inferior interno, mantiene estética */
}


/* Entre la primera y la segunda sección no sumamos margen extra,
   así el espacio visible abajo de la primera es simétrico al de arriba */
.wedding-invitation > .section:nth-of-type(2) {
  margin-top: 0;
}


.invitation-image {
  width: 100%;
  max-width: 500px; /* MÁS PEQUEÑO = MÁS BORDE */
  height: auto;
  object-fit: contain;
  object-position: center;
  display: block;
  box-sizing: border-box;
  min-height: fit-content;
  vertical-align: middle;
  line-height: 0;
}


@media (max-width: 320px) {
  .wedding-invitation {
    gap: 3rem;
  }
  .section {
    padding: 0 2rem;
  }
}

@media (min-width: 321px) and (max-width: 374px) {
  .wedding-invitation {
    gap: 3.5rem;
  }
  .section {
    padding: 0 2.25rem;
  }
}

@media (min-width: 375px) and (max-width: 428px) {
  .wedding-invitation {
    gap: 4rem;
  }
  .section {
    padding: 0 2.5rem;
  }
}

@media (min-width: 429px) and (max-width: 480px) {
  .wedding-invitation {
    gap: 4rem;
  }
  .section {
    padding: 0 2.75rem;
  }
}

@media (min-width: 481px) and (max-width: 767px) {
  .wedding-invitation {
    gap: 4.5rem;
  }
  .section {
    padding: 0 3rem;
  }
}

@media (min-width: 768px) and (max-width: 1024px) {
  .wedding-invitation {
    gap: 5rem;
  }
  .section {
    padding: 0 3.5rem;
  }
}

@media (min-width: 1025px) {
  .wedding-invitation {
    gap: 5rem;
  }
  .section {
    padding: 0 4rem;
  }
}

@media (orientation: landscape) and (max-height: 500px) {
  .section {
    padding: 0 2.5rem;
  }
}

@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .invitation-image {
    -webkit-font-smoothing: antialiased;
    image-rendering: -webkit-optimize-contrast;
    image-rendering: crisp-edges;
  }
}

@supports (padding: max(0px)) {
  .section {
    padding-left: max(2.5rem, env(safe-area-inset-left));
    padding-right: max(2.5rem, env(safe-area-inset-right));
  }
  .section:first-child {
    padding-top: max(1rem, env(safe-area-inset-top));
    padding-bottom: max(1rem, env(safe-area-inset-bottom));
  }
}

@supports (padding: max(0px)) {
  .scroll-indicator {
    bottom: max(0.75rem, calc(env(safe-area-inset-bottom) + 0.5rem));
  }
}

@media (max-height: 700px) {
  .scroll-indicator {
    bottom: 0.75rem;
  }
}
</style>