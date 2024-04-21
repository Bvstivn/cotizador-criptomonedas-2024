<template>
    <div class="contenedor">
      <h1 class="titulo">Cotizador de <span>Cirptomonedas</span></h1>
    </div>
    <div class="contenido">
        <Alerta v-if="error">
            {{ error }}
        </Alerta>
      <form class="formulario"
        @submit.prevent="cotizarCripto">
        <div class="campo">
          <label for="moneda">Moneda:</label>
          <select v-model="cotizar.moneda" id="moneda">
              <option value="">-- Selecciona --</option>
              <option v-for="moneda in monedas" :value="moneda.codigo">{{ moneda.texto }}</option>
          </select>
        </div>

        <div class="campo">
          <label for="cripto">Criptomoneda:</label>
          <select id="cripto">
              <option value="">-- Selecciona --</option>
              <option v-for="criptomoneda in criptomonedas" :value="criptomoneda.CoinInfo.Name">{{ criptomoneda.CoinInfo.FullName }}</option>
          </select>
        </div>

        <input type="submit" value="Cotizar">
      </form>
    </div>
  </template>

<script setup>
//Vue
import { ref, reactive, onMounted } from "vue";
//Components
import Alerta from './components/Alera.vue';

//onMounted
onMounted(() => {
    const url = 'https://min-api.cryptocompare.com/data/top/mktcapfull?limit=10&tsym=USD';
    fetch(url)
    .then(respuesta => respuesta.json())
    .then(({Data}) => criptomonedas.value = Data);
});

//Data
const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
]);
const criptomonedas = ref([]);
const cotizar = reactive({
    moneda: '',
    criptomoneda: ''
});
const error = ref('');

//Methods
const cotizarCripto = () => {
    //Validar que cotizar este lleno
    if(Object.values(cotizar).includes('')){
        error.value = 'Todos los campos son obligatorios';
        return
    };
    error.value = '';
};

</script>
