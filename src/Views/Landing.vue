<script setup lang="ts">
// Invitación de Casamiento
</script>

<template>
  <div class="wedding-invitation">
    <!-- Contenido de la invitación -->
    
  </div>
</template>

<style scoped>
.wedding-invitation {
  /* Mobile First Design - Base para todos los dispositivos */
  min-height: 100vh;
  min-height: 100dvh; /* dvh para soportar barras de navegación en móviles */
  width: 100%;
  background-color: #D4C1DB;
  padding: 1rem;
  margin: 0;
  box-sizing: border-box;
  overflow-x: hidden;
  
  /* Tipografía base responsive */
  font-size: clamp(14px, 3.5vw, 16px);
  line-height: 1.6;
}

/* Dispositivos muy pequeños (iPhone SE, Galaxy Fold cerrado, etc.) */
@media (max-width: 320px) {
  .wedding-invitation {
    padding: 0.75rem;
    font-size: 13px;
  }
}

/* Móviles pequeños en vertical (321px - 374px) */
@media (min-width: 321px) and (max-width: 374px) {
  .wedding-invitation {
    padding: 0.875rem;
  }
}

/* Móviles estándar en vertical (375px - 428px) - iPhone 12, 13, 14, Samsung Galaxy S21, etc. */
@media (min-width: 375px) and (max-width: 428px) {
  .wedding-invitation {
    padding: 1rem;
  }
}

/* Móviles grandes en vertical (429px - 480px) - iPhone Pro Max, Samsung Galaxy S21 Ultra, etc. */
@media (min-width: 429px) and (max-width: 480px) {
  .wedding-invitation {
    padding: 1.25rem;
  }
}

/* Móviles en horizontal / Tablets pequeñas en vertical (481px - 767px) */
@media (min-width: 481px) and (max-width: 767px) {
  .wedding-invitation {
    padding: 1.5rem;
    font-size: 15px;
  }
}

/* Tablets en vertical (768px - 1024px) - iPad, Samsung Tab, etc. */
@media (min-width: 768px) and (max-width: 1024px) {
  .wedding-invitation {
    padding: 2rem;
    font-size: 16px;
    max-width: 90%;
    margin: 0 auto;
  }
}

/* Tablets grandes y Desktop pequeño (1025px+) */
@media (min-width: 1025px) {
  .wedding-invitation {
    padding: 2.5rem;
    font-size: 16px;
    max-width: 800px;
    margin: 0 auto;
  }
}

/* Orientación horizontal específica para móviles */
@media (orientation: landscape) and (max-height: 500px) {
  .wedding-invitation {
    padding: 0.75rem 1.5rem;
    min-height: 100vh;
  }
}

/* Soporte para pantallas de alta densidad (Retina) */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .wedding-invitation {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
}

/* Safe area para dispositivos con notch (iPhone X y superiores) */
@supports (padding: max(0px)) {
  .wedding-invitation {
    padding-left: max(1rem, env(safe-area-inset-left));
    padding-right: max(1rem, env(safe-area-inset-right));
    padding-top: max(1rem, env(safe-area-inset-top));
    padding-bottom: max(1rem, env(safe-area-inset-bottom));
  }
}
</style>
