<template>
  <section class="landing-page">
    <!-- Contenedor principal -->
    <div class="container">
      <h1>Práctica Modelado</h1>
    </div>
  </section>
</template>

<script setup>
// No se necesita lógica adicional
</script>

<style scoped>
/* Estilos para la landing page */
.landing-page {
  height: 100vh; /* Ocupa toda la pantalla */
  background-image: url('/src/assets/landing-bg.jpg'); /* Imagen de fondo */
  background-size: cover;
  background-position: center;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
  position: relative;
  z-index: 1;
}

.landing-page::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6); /* Fondo oscuro transparente */
  z-index: -1; /* Se coloca detrás del contenido */
}

.container h1 {
  font-size: 4rem;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #ffffff;
}

/* Responsive para pantallas pequeñas */
@media (max-width: 768px) {
  .container h1 {
    font-size: 2.5rem;
  }
}
</style>