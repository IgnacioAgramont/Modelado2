<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 4</h1>
    <div class="flex-container">
      <Fieldset legend="Enunciado del Ejercicio 4">
        <p>
          Desarrollar el diagrama de flujo para el siguiente juego de datos: el apostador lanza 2 dados y si saca 7 en la suma de los dos dados gana 5 Bs. De otra manera pierde 2 Bs. Simular el juego y determinar cuanto habrá acumulado el apostador luego de los lanzamientos.
        </p>
      </Fieldset>
      <div class="controls">
        <label for="gamesCount">Número de juegos:</label>
        <input type="number" v-model="gamesCount" id="gamesCount" min="1">
        <button @click="simulateGame">Simular Juego</button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <table>
        <thead>
        <tr>
          <th>Lanzamiento</th>
          <th>Dado 1</th>
          <th>Dado 2</th>
          <th>Suma</th>
          <th>Ganancia/Pérdida</th>
          <th>Total Acumulado</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(result, index) in results" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ result.die1 }}</td>
          <td>{{ result.die2 }}</td>
          <td>{{ result.sum }}</td>
          <td>{{ result.gain }}</td>
          <td>{{ result.total }}</td>
        </tr>
        </tbody>
      </table>
      <table>
        <thead>
        <tr>
          <th>Total de Juegos</th>
          <th>Juegos Ganados por la Casa</th>
          <th>Porcentaje Ganado por la Casa</th>
        </tr>
        </thead>
        <tbody>
        <tr>
          <td>{{ gamesCount }}</td>
          <td>{{ houseWins }}</td>
          <td>{{ houseWinPercentage }}</td>
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
      gamesCount: 10,
      houseWins: 0,
      totalAcc: 0,
      NJGC: 0
    };
  },
  computed: {
    houseWinPercentage() {
      if (this.gamesCount > 0) {
        return ((this.houseWins / this.gamesCount) * 100).toFixed(2) + '%';
      }
      return '0%';
    }
  },
  methods: {
    simulateGame() {
      this.results = [];
      let total = 0;
      let NJGC = 0;
      const PJU = 2;
      const CUS7 = 5;

      for (let i = 0; i < this.gamesCount; i++) {
        const die1 = this.rollDie();
        const die2 = this.rollDie();
        const sum = die1 + die2;
        let gain = 0;

        if (sum === 7) {
          gain = CUS7;
        } else {
          gain = -PJU;
          NJGC++;
        }

        total += gain;

        this.results.push({
          die1,
          die2,
          sum,
          gain,
          total
        });
      }

      this.houseWins = NJGC;
    },
    rollDie() {
      return Math.floor(Math.random() * 6) + 1;
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