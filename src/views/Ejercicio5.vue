<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 5</h1>
    <div class="flex-container">
      <Fieldset legend="Enunciado del Ejercicio 7">
        <p>
          Las llegadas de los clientes a una tienda, siguen una distribución uniforme: 2 ± 2 por hora, y cada cliente compra artículos de acuerdo con la siguiente función de probabilidad: <br>
          Artículos: 0, 1, 2, 3 <br>
          Probabilidad: 0.2, 0.3, 0.4, 0.1 <br>
          Mediante el desarrollo de un modelo de simulación, determine la ganancia neta del dueño y la cantidad de artículos vendidos en un día si sus costos por día ascienden a 300 Bs, el costo del artículo es 50 Bs y el precio de venta es 75 Bs.
        </p>
      </Fieldset>
      <div class="controls">
        <label for="simulationDays">Número de Simulaciones:</label>
        <input type="number" v-model="simulationCount" id="simulationDays" min="1">
        <button @click="simulateStore">Simular Tienda</button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <table>
        <thead>
        <tr>
          <th>Simulación</th>
          <th>Personas Entraron</th>
          <th>Personas Compraron</th>
          <th>Artículos Vendidos</th>
          <th>Ganancia Neta</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(result, index) in results" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ result.totalCustomers }}</td>
          <td>{{ result.buyingCustomers }}</td>
          <td>{{ result.totalItemsSold }}</td>
          <td>{{ result.netGain }}</td>
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
      simulationCount: 1
    };
  },
  methods: {
    simulateStore() {
      this.results = [];

      for (let sim = 0; sim < this.simulationCount; sim++) {
        let totalCustomers = 0;
        let buyingCustomers = 0;
        let totalItemsSold = 0;

        // Simulamos un día (24 horas)
        for (let hour = 0; hour < 24; hour++) {
          // Generar el número de clientes usando distribución uniforme (2 ± 2)
          let customers = this.generateUniform(0, 4); // 2 ± 2 clientes por hora
          totalCustomers += customers;

          for (let i = 0; i < customers; i++) {
            // Generar el número de artículos comprados de acuerdo a las probabilidades dadas
            const itemsBought = this.generateItemsBought();

            if (itemsBought > 0) {
              buyingCustomers++;
              totalItemsSold += itemsBought;
            }
          }
        }

        // Calcular ingresos y ganancias
        let totalRevenue = totalItemsSold * 75; // Precio de venta de cada artículo: 75 Bs
        let totalCost = totalItemsSold * 50 + 300; // Costo del artículo: 50 Bs + costos fijos diarios: 300 Bs
        let netGain = totalRevenue - totalCost;

        this.results.push({
          totalCustomers,
          buyingCustomers,
          totalItemsSold,
          netGain
        });
      }
    },

    // Función para generar un número de clientes usando la distribución uniforme (2 ± 2)
    generateUniform(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },

    // Función para generar el número de artículos comprados de acuerdo con las probabilidades
    generateItemsBought() {
      const random = Math.random();

      if (random <= 0.2) {
        return 0; // 20% de probabilidad de no comprar artículos
      } else if (random <= 0.5) {
        return 1; // 30% de probabilidad de comprar 1 artículo
      } else if (random <= 0.9) {
        return 2; // 40% de probabilidad de comprar 2 artículos
      } else {
        return 3; // 10% de probabilidad de comprar 3 artículos
      }
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