<script setup>
import Header from './components/Header.vue';
import Button from './components/Button.vue'
import { ref, reactive, computed, watch } from 'vue';
import { calcularTotalPagar } from './helpers'

const cantidad = ref(10000);
const meses = ref(6)
const total = ref(0);

const MIN = 0;
const MAX = 20000;
const STEP = 100;

//Formatear dinero

const FormatearDinero = (valor) => {
  const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'

  })
  return formatter.format(valor)
}

watch([cantidad, meses], () => {
  total.value = calcularTotalPagar(cantidad.value, meses.value)

})

const pagoMensual = computed(() => {
      return total.value / meses.value
  });

//funciones para el incremento y descremento
function handleChange(e) {
  cantidad.value = +e.target.value;
}

function handleChangeIncremento() {

  const valor = cantidad.value + STEP

  if (valor > MAX) {
    alert('Cantidad no valida');
    return;
  }
  cantidad.value = valor
}

function handleChangeDescremento() {

  const valor = cantidad.value - STEP

  if (valor < MIN) {
    alert('Cantidad no valida');
    return;
  }
  cantidad.value = valor
}

</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">

    <Header />
    <div class="flex justify-between mt-10">

      <Button :operador="'-'" @fn="handleChangeDescremento" />

      <Button :operador="'+'" @fn="handleChangeIncremento" />

    </div>

    <div class="my-5">

      <input type='range' class="w-full h-6 bg-gray-200 accent-lime-500 hover:accent-lime-600" @input="handleChange"
        :min="MIN" :max="MAX" :step="STEP" :value="cantidad" />

      <p class='text-center my-10 text-5xl font-extrabold text-indigo-600'>
        {{ FormatearDinero(cantidad) }}
      </p>

      <h2 class='text-2xl font-extrabold text-gray-500 text-center'>

        Elige un <span class='text-indigo-600'>Plazo </span> a pagar

      </h2>

      <select
        class='mt-5 w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500'
        v-model.number="meses">
        <option value="6">6 Meses</option>
        <option value="12">12 Meses</option>
        <option value="24">24 Meses</option>
      </select>

    </div>

    <div v-if="total > 0" class='my-5 space-y-3 bg-gray-50 p-5'>

      <h2 class='text-2xl font-extrabold text-gray-500 text-center'>
        Resumen <span class='text-indigo-600'>de pagos </span>
      </h2>

      <p class='text-xl text-gray-500 text-center font-bold'>{{ meses }} Meses</p>
      <p class='text-xl text-gray-500 text-center font-bold'> {{ FormatearDinero(total) }} Total a pagar</p>
      <p class='text-xl text-gray-500 text-center font-bold'> {{ FormatearDinero(pagoMensual) }} Mensuales</p>

    </div>
    <p v-else class="text-center">Añade una cantidad y un plazo a pagar</p>

  </div>

</template>

