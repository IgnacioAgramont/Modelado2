<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 6</h1>
    <div class="flex-container">
      <Fieldset legend="Enunciado del Ejercicio 5">
        <p>
          Un granjero tiene una gallina que pone huevos a una razón Poisson con media de 2 huevos/día. El 20% de los huevos se rompen, del 30% de ellos nacen pollos y el resto permanecen como huevos. De los pollos, el 20% muere y el 80% sobrevive. Simule este sistema y determine el ingreso promedio del granjero si cada huevo lo vende en 2 Bs y cada pollo en 30 Bs.
        </p>
      </Fieldset>
      <div class="controls">
        <label for="simulationDays">Número de días:</label>
        <input type="number" v-model="simulationDays" id="simulationDays" min="1">
        <button @click="simulateFarm">Simular Granja</button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <p><strong>Total de Ganancia de los {{ simulationDays }} días:</strong> {{ totalIncome }} Bs.</p>
      <p><strong>Huevos Puestos en el total de días:</strong> {{ totalEggsLaid }} </p>
      <p><strong>Pollos Nacidos en el total de días:</strong> {{ totalChicksBorn }} </p>
      <p><strong>Huevos Rotos en el total de días:</strong> {{ totalEggsBroken }} </p>
      <p><strong>Pollos Muertos en el total de días:</strong> {{ totalChicksDied }} </p>
      <p><strong>Total de Pollos vendidos:</strong> {{ totalChicksSold }}</p>
      <p><strong>Total de Huevos vendidos:</strong> {{ totalEggsSold }}</p>
      <table>
        <thead>
        <tr>
          <th>Día</th>
          <th>Huevos Puestos</th>
          <th>Huevos Rotos</th>
          <th>Pollos Nacidos</th>
          <th>Pollos Muertos</th>
          <th>Ingreso del Día (Bs)</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(result, index) in results" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ result.eggsLaid }}</td>
          <td>{{ result.eggsBroken }}</td>
          <td>{{ result.chicksBorn }}</td>
          <td>{{ result.chicksDied }}</td>
          <td>{{ result.dailyIncome }}</td>
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
      results: [],
      simulationDays: 5
    };
  },
  computed: {
    totalIncome() {
      return this.results.reduce((acc, result) => acc + result.dailyIncome, 0);
    },
    totalEggsLaid() {
      return this.results.reduce((acc, result) => acc + result.eggsLaid, 0);
    },
    totalChicksBorn() {
      return this.results.reduce((acc, result) => acc + result.chicksBorn, 0);
    },
    totalEggsBroken() {
      return this.results.reduce((acc, result) => acc + result.eggsBroken, 0);
    },
    totalChicksDied() {
      return this.results.reduce((acc, result) => acc + result.chicksDied, 0);
    },
    totalChicksSold() {
      return this.results.reduce((acc, result) => acc + (result.chicksBorn - result.chicksDied), 0);
    },
    totalEggsSold() {
      return this.results.reduce((acc, result) => acc + (result.eggsLaid - result.eggsBroken - result.chicksBorn), 0);
    }
  },
  methods: {
    simulateFarm() {
      this.results = [];
      for (let day = 1; day <= this.simulationDays; day++) {
        const eggsLaid = this.poissonRandom(2); // Media de 2 huevos por día
        const eggsBroken = Math.round(eggsLaid * 0.2); // El 20% de los huevos se rompen
        const chicksBorn = Math.round((eggsLaid - eggsBroken) * 0.3); // El 30% de los huevos restantes nacen pollos
        let chicksDied = 0;

        for (let i = 0; i < chicksBorn; i++) {
          if (Math.random() < 0.2) { // El 20% de los pollos mueren
            chicksDied++;
          }
        }

        const dailyIncome = (eggsLaid - eggsBroken - chicksBorn) * 2 + (chicksBorn - chicksDied) * 30; // Venta de huevos y pollos
        this.results.push({
          eggsLaid,
          eggsBroken,
          chicksBorn,
          chicksDied,
          dailyIncome
        });
      }
    },
    poissonRandom(lambda) {
      let L = Math.exp(-lambda);
      let k = 0;
      let p = 1;
      do {
        k++;
        p *= Math.random();
      } while (p > L);
      return k - 1;
    }
  }
}
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
  padding-top: 100px;
}

/* Título blanco y centrado */
.page-title {
  font-size: 50px;
  color: white;
  text-align: center;
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
  border: none;
  box-shadow: none;
  text-align: center;
  color: #003a8c;
  width: 50%;
}

/* Controles con fondo estilizado y sin borde */
.controls {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 10px;
  border: none;
  box-shadow: none;
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