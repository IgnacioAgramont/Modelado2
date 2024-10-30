<template>
  <div class="card">
    <fieldset class="fieldset">
      <legend class="fieldset-legend">{{ legend }}</legend>
      <slot></slot>
    </fieldset>
  </div>
</template>

<script>
export default {
  props: {
    legend: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.card {
  margin: 0;
  width: 450px;
  background-color: rgb(233, 228, 238);
  border-radius: 25px;
  box-shadow: 0 0 110px #190707;
}

/* Elimina el borde y ajusta padding */
.fieldset {
  border: none; /* Elimina el borde del fieldset */
  border-radius: 25px;
  padding: 10px 20px;
  color: rgb(0, 0, 0);
  font-weight: bold;
  font-size: 1.2em;
}

/* Elimina cualquier padding extra en el legend */
.fieldset-legend {
  font-weight: bold;
  font-size: 1.2em;
  padding: 0; /* Elimina el padding extra */
  color: rgb(25, 20, 27);
}
</style>