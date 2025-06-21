<template>
  <div class="q-pa-md">
    <q-card flat bordered class="q-pa-lg">
      <div class="text-h5 text-center q-mb-md">💱 Conversión de Monedas</div>

      <q-form @submit.prevent="convertCurrency" ref="form">
        <!-- Campo de Monto -->
        <q-input
          v-model="amount"
          label="Monto a convertir"
          outlined
          class="q-mb-md"
          :rules="[val => val !== '' && !isNaN(val) && Number(val) > 0 || 'Monto inválido']"
        />

        <!-- Selectores de Moneda -->
        <div class="row q-gutter-md q-mb-md">
          <q-select
            v-model="from"
            :options="currencyOptions"
            label="Desde"
            emit-value
            map-options
            outlined
            class="col"
            :rules="[val => !!val || 'Seleccione moneda de origen']"
          />
          <q-btn icon="swap_horiz" round flat @click="swapCurrencies" />
          <q-select
            v-model="to"
            :options="currencyOptions"
            label="Hacia"
            emit-value
            map-options
            outlined
            class="col"
            :rules="[val => !!val || 'Seleccione moneda de destino']"
          />
        </div>

        <!-- Botón de Conversión -->
        <q-btn label="Convertir" color="primary" type="submit" />

        <!-- Resultado -->
        <div v-if="result !== null" class="q-mt-md text-center text-subtitle1">
          <strong>{{ amount }} {{ from }}</strong> equivalen a
          <strong>{{ result }} {{ to }}</strong>
        </div>
      </q-form>
    </q-card>
  </div>
</template>

<script>
import { ref } from 'vue'
import axios from 'axios'

export default {
  name: 'CurrencyConverter',
  setup() {
    const amount = ref('')
    const from = ref(null)
    const to = ref(null)
    const result = ref(null)
    const currencyOptions = ref([])
    const form = ref(null)

    const loadCurrencies = async () => {
      try {
        const res = await axios.get('https://api.frankfurter.app/currencies')
        currencyOptions.value = Object.entries(res.data).map(([code, name]) => ({
          label: `${name} (${code})`,
          value: code
        }))
      } catch (error) {
        console.error(error)
        this.$q.notify({
          type: 'negative',
          message: 'No se pudieron cargar las monedas'
        })
      }
    }

    const convertCurrency = async () => {
      const isValid = await form.value.validate()
      if (!isValid) {
        this.$q.notify({
          type: 'negative',
          message: 'Por favor complete todos los campos correctamente'
        })
        return
      }

      if (from.value === to.value) {
        result.value = Number(amount.value)
        return
      }

      try {
        const url = `https://api.frankfurter.app/latest?amount=${amount.value}&from=${from.value}&to=${to.value}`
        const res = await axios.get(url)
        result.value = res.data.rates[to.value]
      } catch (error) {
        console.error(error)
        this.$q.notify({
          type: 'negative',
          message: 'No se pudo realizar la conversión'
        })
      }
    }

    const swapCurrencies = () => {
      const temp = from.value
      from.value = to.value
      to.value = temp
      result.value = null
    }

    loadCurrencies()

    return {
      amount,
      from,
      to,
      result,
      currencyOptions,
      form,
      convertCurrency,
      swapCurrencies
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
