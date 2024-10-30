<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 3</h1>
    <div class="flex-container">
      <Fieldset legend="Enunciado del Ejercicio 3">
        <p>
          Max Z = 2X1 + 3X2 - X3<br>
          Sujeto a:<br>
          X1 + X2 &gt;= 2<br>
          1 &lt;= X3 &lt;= 2<br>
          X2 &gt;= 0 (Entero)<br>
          0 &lt;= X1 &lt;= 10
        </p>
      </Fieldset>
      <div class="controls">
        <label for="iterations">Iteraciones:</label>
        <input type="number" v-model="NUM_ITERACIONES" id="iterations" min="1">
        <button @click="simulateMonteCarlo">Simular</button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <table>
        <thead>
        <tr>
          <th>Mejor Z Encontrado</th>
          <th>Iteración de Mejor Solución</th>
        </tr>
        </thead>
        <tbody>
        <tr>
          <td>{{ Z }}</td>
          <td>{{ iteracion_mejor_solucion + 1 }}</td>
        </tr>
        </tbody>
      </table>
      <table>
        <thead>
        <tr>
          <th>Iteración</th>
          <th>X1</th>
          <th>X2</th>
          <th>X3</th>
          <th>Z (Función Objetivo)</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(result, index) in results" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ result.x1 }}</td>
          <td>{{ result.x2 }}</td>
          <td>{{ result.x3 }}</td>
          <td>{{ result.z }}</td>
        </tr>
        </tbody>
      </table>
    </Fieldset>
  </div>
</template>

<script>
import Fieldset from '../components/CardProblem.vue';

export default {
  components: {
    Fieldset
  },
  data() {
    return {
      NUM_ITERACIONES: 1000,
      results: [],
      mejor_solucion: null,
      Z: -Infinity,
      iteracion_mejor_solucion: null,
    };
  },

  methods: {
    generar_rnd(min, max) {
      return Math.random() * (max - min) + min; // Cambia la generación de número aleatorio
    },

    simulateMonteCarlo() {
      this.mejor_solucion = null;
      this.Z = -Infinity; // Establece Z a infinito negativo para la maximización
      this.iteracion_mejor_solucion = null;
      this.results = [];

      for (let i = 0; i < this.NUM_ITERACIONES; i++) {
        // Generación de X1 y X2 en el rango correcto
        const x1 = this.generar_rnd(0, 10);
        const x2 = Math.floor(this.generar_rnd(0, 100)); // Asegúrate de redondear a entero
        const x3 = this.generar_rnd(1, 2); // Genera X3 en el rango [1, 2]

        // Validación de la restricción
        const isValid = (x1 + x2) <= 20; // Cambié la condición para que coincida con el algoritmo original
        const z = isValid ? (2 * x1 + 3 * x2 - x3) : null;

        // Actualizar mejor solución si es válida y mejora Z
        if (isValid && z > this.Z) {
          this.Z = z;
          this.mejor_solucion = { x1, x2, x3, z };
          this.iteracion_mejor_solucion = i; // Guarda la iteración actual
        }

        // Almacenar el resultado para cada iteración
        this.results.push({ x1, x2, x3, z: isValid ? z : 'Invalido' });
      }
    },
  },
};
</script>

<style scoped>
/* Estilo del fondo */
.page {
  background: url('https://www.wallpaperbetter.com/wallpaper/612/299/997/beautiful-gradient-background-1080P-wallpaper.jpg') no-repeat center center fixed;
  background-size: cover;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding-top: 100px; /* Ajuste para evitar que el navbar lo tape */
}

/* Título blanco y centrado */
.page-title {
  font-size: 50px;
  color: white;
  text-align: center;
  z-index: 1;
  margin-bottom: 20px;
}

/* Contenedor centralizado */
.flex-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  width: 100%;
}

/* Caja del enunciado sin bordes y sin sombras */
.enunciado {
  background-color: #e6f7ff;
  padding: 15px;
  border-radius: 10px;
  border: none; /* Quitar el borde */
  box-shadow: none; /* Quitar sombras */
  text-align: center;
  color: #003a8c;
  width: 50%;
}

/* Controles con fondo estilizado y sin borde */
.controls {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 10px;
  border: none; /* Sin borde */
  box-shadow: none; /* Sin sombra */
  display: flex;
  flex-direction: column;
  gap: 15px;
  align-items: center;
  width: 50%;
}

label {
  color: #004080;
  font-size: 18px;
  font-weight: bold;
}

/* Botón estilizado */
button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

/* Tabla de resultados sin bordes extra */
.results-container {
  width: 100%;
  background-color: #fff5e6;
  padding: 20px;
  margin: 20px auto;
  border-radius: 10px;
  border: none; /* Sin borde */
  box-shadow: none; /* Sin sombra */
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: center;
}

th, td {
  border: 1px solid #dee2e6;
  padding: 12px;
}

th {
  background-color: #ff9800;
  color: white;
}

td {
  background-color: #ffecb3;
}

input {
  border-radius: 5px;
  padding: 10px;
  font-size: 16px;
  text-align: center;
  border: 1px solid #ccc;
}
</style>