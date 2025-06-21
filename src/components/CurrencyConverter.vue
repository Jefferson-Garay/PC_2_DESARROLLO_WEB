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

      <!-- Selects de moneda + botón swap -->
      <div class="row items-center q-gutter-md q-mb-md">
        <q-select
          v-model="from"
          :options="currencyOptions"
          label="Desde"
          outlined
          class="col"
        />
        <q-btn
          icon="swap_horiz"
          round
          flat
          @click="swapCurrencies"
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
      amount: null,     // vacía
      from: null,       // vacía
      to: null,         // vacía
      currencyOptions: [
        { label: 'Dólar (USD)', value: 'USD' },
        { label: 'Euro (EUR)', value: 'EUR' },
        { label: 'Yen (JPY)', value: 'JPY' },
        { label: 'Libra (GBP)', value: 'GBP' }
      ]
    }
  },
  methods: {
    swapCurrencies() {
      const temp = this.from
      this.from = this.to
      this.to = temp
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
