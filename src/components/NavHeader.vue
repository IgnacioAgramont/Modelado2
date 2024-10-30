<template>
  <header class="navbar">
    <nav class="navbar-container">
      <router-link to="/" class="logo">Práctica Modelado</router-link>
      <ul class="nav-links">
        <li><router-link to="/">Home</router-link></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle">Ejercicios</a>
          <ul class="dropdown-menu">
            <li><router-link to="/ejercicio1">Ejercicio 1</router-link></li>
            <li><router-link to="/ejercicio2">Ejercicio 2</router-link></li>
            <li><router-link to="/ejercicio3">Ejercicio 3</router-link></li>
            <li><router-link to="/ejercicio4">Ejercicio 4</router-link></li>
            <li><router-link to="/ejercicio5">Ejercicio 5</router-link></li>
            <li><router-link to="/ejercicio6">Ejercicio 6</router-link></li>
            <li><router-link to="/ejercicio7">Ejercicio 7</router-link></li>
<!--            <li><router-link to="/ejercicio8">Ejercicio 8</router-link></li>-->
          </ul>
        </li>
<!--        <li><router-link to="/estudiante">Estudiante</router-link></li>-->
      </ul>
    </nav>
  </header>
</template>

<script setup>
// No es necesario incluir lógica adicional en este componente.
</script>

<style scoped>
/* Navbar moderno con fondo translúcido y estilo más limpio */
.navbar {
  background: rgba(0, 0, 0, 0.8); /* Fondo translúcido */
  color: white;
  padding: 1rem 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Sombra ligera */
  position: fixed; /* Navbar siempre visible en la parte superior */
  width: 100%;
  top: 0;
  left: 0;
  z-index: 1000;
}

.navbar-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.8rem;
  font-weight: bold;
  color: white;
  text-decoration: none;
}

.nav-links {
  list-style: none;
  display: flex;
  gap: 1.5rem;
}

.nav-links a {
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  transition: background-color 0.3s ease;
}

.nav-links a:hover {
  background-color: rgba(255, 255, 255, 0.1); /* Efecto hover más suave */
}

.dropdown {
  position: relative;
}

.dropdown-toggle {
  cursor: pointer;
  color: white;
  padding: 0.5rem 1rem;
}

.dropdown-menu {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  background-color: rgba(0, 0, 0, 0.9);
  padding: 1rem;
  list-style: none;
  border-radius: 8px; /* Bordes redondeados */
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); /* Sombra para darle profundidad */
  min-width: 200px;
}

.dropdown-menu li {
  margin-bottom: 0.5rem;
}

.dropdown-menu li:last-child {
  margin-bottom: 0;
}

.dropdown-menu a {
  color: white;
  padding: 0.5rem 1rem;
  display: block;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.dropdown-menu a:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.dropdown:hover .dropdown-menu {
  display: block;
  animation: dropdownFadeIn 0.3s ease-in-out;
}

@keyframes dropdownFadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Estilos responsive para móviles */
@media (max-width: 768px) {
  .navbar-container {
    flex-direction: column;
    align-items: flex-start;
  }

  .nav-links {
    flex-direction: column;
    width: 100%;
    text-align: left;
  }

  .nav-links li {
    width: 100%;
  }

  .nav-links a {
    padding: 1rem;
    width: 100%;
  }
}
</style>