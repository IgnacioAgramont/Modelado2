<template>
  <div class="page">
    <h1 class="page-title">Ejercicio 7
    </h1>
    <div class="flex-container">
      <Fieldset legend="Enunciado del Ejercicio 6">
        <p>
          La demanda de azúcar en una tienda sigue una distribución exponencial con media de 100 Kg/día. El dueño de la tienda revisa el inventario cada 7 días y hace un pedido a la planta igual a la capacidad de la bodega menos la cantidad de azúcar que tiene disponible en ese momento: la entrega no es inmediata y sigue una distribución uniforme entre 1 y 3 días. La demanda no surtida por falta de existencias representa ventas perdidas. La capacidad de almacenamiento de la bodega es de 700 Kgrs. El costo de ordenar es de 100 Bs./orden. El costo de llevar el inventario es igual a 0.1 Bs./Kg, el costo de adquisición es igual a 3.5 Bs/Kgr y el precio de venta igual 5 Bs/Kgr. Determinar el comportamiento del inventario a lo largo del tiempo, el costo y la ganancia neta, la demanda insatisfecha para un horizonte de dos meses. Será la capacidad del almacén suficiente?
        </p>
      </Fieldset>
      <div class="controls">
        <label for="simulationDays">Número de simulaciones:</label>
        <input type="number" v-model="simulationDays" id="simulationDays" min="1">
        <button @click="simulateStore">Simular Tienda</button>
      </div>
    </div>
    <Fieldset v-if="results.length" legend="Resultados" class="results-container">
      <table>
        <thead>
        <tr>
          <th>Día</th>
          <th>Inventario Inicial</th>
          <th>Demanda</th>
          <th>Inventario Final</th>
          <th>Demanda Insatisfecha</th>
          <th>Costo de Ordenar</th>
          <th>Costo de Inventario</th>
          <th>Ganancia Neta</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(result, index) in results" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ result.initialInventory }}</td>
          <td>{{ result.demand }}</td>
          <td>{{ result.finalInventory }}</td>
          <td>{{ result.unmetDemand }}</td>
          <td>{{ result.orderingCost }}</td>
          <td>{{ result.inventoryCost }}</td>
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
      simulationDays: 60, // Valor por defecto para dos meses
      currentInventory: 700,
      deliveryTime: 0,
      totalOrderCost: 0,
      totalHoldingCost: 0,
      totalDemandUnsatisfied: 0,
      totalNetProfit: 0
    };
  },
  methods: {
    simulateStore() {
      this.results = [];
      let totalInventoryCost = 0;
      let totalOrderCost = 0;
      let totalNetProfit = 0;
      let unmetDemandTotal = 0;

      for (let day = 1; day <= this.simulationDays; day++) {
        let demand = Math.round(Math.random() * 100); // Demanda del día
        let initialInventory = this.currentInventory;
        let unmetDemand = 0;
        let orderingCost = 0;
        let inventoryCost = Math.round(this.currentInventory * 0.1); // Costo de mantener inventario
        let netGain = 0;

        // Pedido cada 7 días
        if (day % 7 === 0 || this.currentInventory === 0) {
          orderingCost = 100; // Costo de la orden
          this.deliveryTime = Math.floor(Math.random() * 3) + 1; // Tiempo de entrega
        }

        // Manejo de tiempo de entrega y recepción de inventario
        if (this.deliveryTime > 0) {
          this.deliveryTime--;
          if (this.deliveryTime === 0) {
            this.currentInventory = 700; // Reabastece el inventario a su capacidad
          }
        }

        // Actualización de inventario y demanda insatisfecha
        if (demand > this.currentInventory) {
          unmetDemand = demand - this.currentInventory; // Ventas perdidas
          this.currentInventory = 0;
        } else {
          this.currentInventory -= demand; // Inventario disminuye según la demanda
        }

        // Cálculo de ganancia neta
        netGain = (demand - unmetDemand) * 5 - demand * 3.5 - orderingCost - inventoryCost;

        this.results.push({
          initialInventory,
          demand,
          finalInventory: this.currentInventory,
          unmetDemand,
          orderingCost,
          inventoryCost,
          netGain
        });

        // Acumulación de costos y ganancias totales
        totalInventoryCost += inventoryCost;
        totalOrderCost += orderingCost;
        totalNetProfit += netGain;
        unmetDemandTotal += unmetDemand;
      }

      this.totalHoldingCost = totalInventoryCost;
      this.totalOrderCost = totalOrderCost;
      this.totalNetProfit = totalNetProfit;
      this.totalDemandUnsatisfied = unmetDemandTotal;
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