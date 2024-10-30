<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 2</h1>
    <div class="flex-container">
      <Fieldset legend="Modelo Demográfico para Bolivia 2012-2023">
        <p>
          ¿Cómo se puede determinar la población de Bolivia en base a los resultados del censo 2012?
        </p>
      </Fieldset>
      <div class="controls">
        <label for="initialPopulation">Población Inicial:</label>
        <input type="number" v-model="initialPopulation" id="initialPopulation" min="1" placeholder="Ejemplo: 10059856">

        <button @click="simulatePopulationGrowth" :disabled="!initialPopulation">Simular Crecimiento Poblacional
        </button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <table>
        <thead>
        <tr>
          <th>Año</th>
          <th>Nacimientos</th>
          <th>Muertes</th>
          <th>Población</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="result in results" :key="result.year">
          <td>{{ result.year }}</td>
          <td>{{ formatNumber(result.births) }}</td>
          <td>{{ formatNumber(result.deaths) }}</td>
          <td>{{ formatNumber(result.population) }}</td>
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
      initialPopulation: 10059856,
      results: []
    };
  },
  methods: {
    simulatePopulationGrowth() {
      let CT = 2013; // Año inicial
      const T = 2024; // Año final
      const TN = 0.02493; // Tasa de nacimientos
      const TM = 0.00743; // Tasa de mortalidad
      let PB = Number(this.initialPopulation); // Población inicial ingresada por el usuario

      this.results = [];

      while (CT <= T) {
        let NAC = Math.round(PB * TN);
        let MUE = Math.round(PB * TM);

        this.results.push({
          year: CT,
          births: NAC,
          deaths: MUE,
          population: Math.round(PB)
        });

        PB = PB + NAC - MUE;
        CT++;
      }

      console.log(`Valor final de PB: ${PB}`);
    },
    formatNumber(number) {
      return new Intl.NumberFormat().format(number);
    }
  }
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

button:disabled {
  background-color: #3e0669;
  cursor: not-allowed;
}

/* Tabla de resultados sin bordes extra */
.results-container {
  width: 100%;
  background-color: #fff5e6;
  padding: 20px;
  margin: 20px auto;
  border-radius: 10px;
  border: none;
  box-shadow: none;
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