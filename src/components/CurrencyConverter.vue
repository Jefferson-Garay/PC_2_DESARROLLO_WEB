<template>
  <div class="q-pa-md">
    <q-card flat bordered class="q-pa-lg">
      <div class="text-h5 text-center q-mb-md">💱 Conversión de Monedas</div>

      <!-- Campo de monto -->
      <q-input
        v-model.number="amount"
        label="Monto a convertir"
        type="number"
        outlined
        class="q-mb-md"
      />

      <!-- Selects de moneda -->
      <div class="row q-gutter-md q-mb-md">
        <q-select
          v-model="from"
          :options="currencyOptions"
          label="Desde"
          outlined
          class="col"
        />
        <q-select
          v-model="to"
          :options="currencyOptions"
          label="Hacia"
          outlined
          class="col"
        />
      </div>
    </q-card>
  </div>
</template>

<script>
export default {
  name: 'CurrencyConverter',
  data() {
    return {
      amount: null,
      from: null,
      to: null,
      currencyOptions: [
        { label: 'Dólar (USD)', value: 'USD' },
        { label: 'Euro (EUR)', value: 'EUR' },
        { label: 'Yen (JPY)', value: 'JPY' },
        { label: 'Libra (GBP)', value: 'GBP' }
      ]
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
