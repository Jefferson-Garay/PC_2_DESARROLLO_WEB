<template>
  <div class="q-pa-md">
    <q-card flat bordered class="q-pa-lg">
      <div class="text-h5 text-center q-mb-md">💱 Conversión de Monedas</div>

      <!-- Campo para ingresar monto -->
      <q-input
        v-model.number="amount"
        label="Monto a convertir"
        type="number"
        outlined
        class="q-mb-md"
      />
    </q-card>
  </div>
</template>

<script>
export default {
  name: 'CurrencyConverter',
  data() {
    return {
      amount: null
    }
  }
}
</script>

<style scoped>
.q-card {
  max-width: 500px;
  margin: auto;
}
</style>
