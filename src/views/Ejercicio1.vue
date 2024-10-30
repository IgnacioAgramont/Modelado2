<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 1</h1>
    <div class="flex-container">
      <Fieldset legend="Enunciado del Ejercicio 1" class="enunciado">
        <p>
          ¿Cuál será el capital en un depósito a plazo fijo con una tasa de interés constante?
        </p>
      </Fieldset>
      <div class="controls">
        <label for="capitalInicial">Capital Inicial ($):</label>
        <input type="number" v-model="capitalInicial" id="capitalInicial" min="1">
        <label for="tiempoDeposito">Tiempo en años:</label>
        <input type="number" v-model="tiempoDeposito" id="tiempoDeposito" min="1">
        <button @click="simulateInvestment">Simular Depósito</button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <table>
        <thead>
        <tr>
          <th>Año</th>
          <th>Interés ($/año)</th>
          <th>Capital ($)</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(result, index) in results" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ result.interest }}</td>
          <td>{{ result.capital }}</td>
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
      capitalInicial: 1000,
      tiempoDeposito: 5,
      results: []
    };
  },
  methods: {
    simulateInvestment() {
      this.results = [];
      let capital = this.capitalInicial;
      const interestRate = 0.035; // 3.5% tasa de interés anual

      for (let year = 1; year <= this.tiempoDeposito; year++) {
        let interest = capital * interestRate;
        capital += interest;
        this.results.push({
          interest: interest.toFixed(2),
          capital: capital.toFixed(2)
        });
      }
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

/* Tabla de resultados sin bordes extra */
.results-container {
  width: 80%;
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