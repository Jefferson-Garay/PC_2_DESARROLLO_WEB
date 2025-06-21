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

      <!-- Selects y botón de swap -->
      <div class="row items-center q-gutter-md q-mb-md">
        <q-select
          v-model="from"
          :options="currencyOptions"
          label="Desde"
          option-label="label"
          option-value="value"
          emit-value
          map-options
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
          option-label="label"
          option-value="value"
          emit-value
          map-options
          outlined
          class="col"
        />
      </div>

      <!-- Botón de conversión -->
      <q-btn label="Convertir" color="primary" @click="convertCurrency" class="q-mb-md" />

      <!-- Resultado -->
      <div v-if="result !== null" class="q-mt-md text-center text-subtitle1">
        <strong>{{ amount }} {{ from }}</strong> equivalen a
        <strong>{{ result }} {{ to }}</strong>
      </div>
    </q-card>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'CurrencyConverter',
  data() {
    return {
      amount: null,
      from: null,
      to: null,
      result: null,
      currencyOptions: []
    }
  },
  methods: {
    swapCurrencies() {
      const temp = this.from
      this.from = this.to
      this.to = temp
    },
    async loadCurrencies() {
      try {
        const res = await axios.get('https://api.frankfurter.app/currencies')
        this.currencyOptions = Object.entries(res.data).map(([code, name]) => ({
          label: `${name} (${code})`,
          value: code
        }))
      } catch (error) {
        console.error('Error cargando monedas:', error)
        this.$q.notify({
          type: 'negative',
          message: 'No se pudo cargar la lista de monedas.'
        })
      }
    },
    async convertCurrency() {
      if (!this.amount || this.amount <= 0) {
        this.$q.notify({
          type: 'negative',
          message: 'Ingresa un monto válido mayor que 0.'
        })
        return
      }

      if (!this.from || !this.to) {
        this.$q.notify({
          type: 'negative',
          message: 'Selecciona ambas monedas.'
        })
        return
      }

      try {
        const url = `https://api.frankfurter.app/latest?amount=${this.amount}&from=${this.from}&to=${this.to}`
        const res = await axios.get(url)
        this.result = res.data.rates[this.to]
      } catch (error) {
        console.error('Error en la conversión:', error)
        this.$q.notify({
          type: 'negative',
          message: 'Ocurrió un error al convertir.'
        })
      }
    }
  },
  mounted() {
    this.loadCurrencies()
  }
}
</script>

<style scoped>
.q-card {
  max-width: 500px;
  margin: auto;
}
</style>
