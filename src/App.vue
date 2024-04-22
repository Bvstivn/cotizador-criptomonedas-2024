<template>
  <div class="contenedor">
    <h1 class="titulo">Cotizador de <span>Cirptomonedas</span></h1>
  </div>
  <div class="contenido">
    <Alerta v-if="error">
      {{ error }}
    </Alerta>
    <form class="formulario" @submit.prevent="cotizarCripto">
      <div class="campo">
        <label for="moneda">Moneda:</label>
        <select v-model="cotizar.moneda" id="moneda">
          <option value="">-- Selecciona --</option>
          <option v-for="moneda in monedas" :value="moneda.codigo">
            {{ moneda.texto }}
          </option>
        </select>
      </div>

      <div class="campo">
        <label for="cripto">Criptomoneda:</label>
        <select id="cripto">
          <option value="">-- Selecciona --</option>
          <option
            v-for="criptomoneda in criptomonedas"
            :value="criptomoneda.CoinInfo.Name"
          >
            {{ criptomoneda.CoinInfo.FullName }}
          </option>
        </select>
      </div>

      <input type="submit" value="Cotizar" />
    </form>

    <Spinner v-if="cargando" />

    <div v-if="mostrarResultado" class="contenedor-resultado">
      <h2>Cotización</h2>
      <div class="resultado">
        <img
          :src="'https://cryptocompare.com/' + cotizacion.IMAGEURL"
          alt="imagen cripto"
        />
        <div>
          <p>
            El precio es de: <span>{{ cotizacion.PRICE }}</span>
          </p>
          <p>
            El precio más alto del día: <span>{{ cotizacion.HIGHDAY }}</span>
          </p>
          <p>
            El precio más bajo del día: <span>{{ cotizacion.LOWDAY }}</span>
          </p>
          <p>
            Variación últimas 24 horas:
            <span>{{ cotizacion.CHANGEPCT24HOUR }}%</span>
          </p>
          <p>
            Última Actualización: <span>{{ cotizacion.LASTUPDATE }}</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
//Vue
import { reactive } from "vue";
//Components
import Alerta from "./components/Alera.vue";
import Spinner from "./components/Spinner.vue";
//Composables
import useCripto from "./composables/useCripto";

//Data
const { monedas, criptomonedas, error, cotizacion, cargando, obtenerCotizacion, mostrarResultado } = useCripto();

const cotizar = reactive({
  moneda: "",
  criptomoneda: "",
});

//Methods
const cotizarCripto = () => {
  //Validar que cotizar este lleno
  if (Object.values(cotizar).includes("")) {
    error.value = "Todos los campos son obligatorios";
    return;
  }
  error.value = "";
  obtenerCotizacion(cotizar);
};
</script>
