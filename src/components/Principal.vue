<template>
  <div class="bg-gray-100 min-h-screen py-8">
    <div class="container mx-auto px-4">
      <!-- Título -->
      <h1 class="text-3xl font-bold text-center text-blue-600 mb-8">
        Calculadora de Consumo Eléctrico y Paneles Solares
      </h1>

      <!-- Ingresar datos de electrodomésticos -->
      <div class="bg-white shadow-md rounded-lg p-6 mb-8">
        <h2 class="text-xl font-semibold mb-4 text-gray-700">
          Ingrese los datos de sus electrodomésticos:
        </h2>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <ElectrodomesticoItem
            v-for="(electrodomestico, index) in electrodomesticos"
            :key="index"
            :electrodomestico="electrodomestico"
            :index="index"
            @update="updateElectrodomestico"
          />
        </div>

        <!-- Componente de información del panel solar -->
        <InfoPanelSolar @update:panel-info="updatePanelInfo" />

        <!-- Botón para iniciar el cálculo -->
        <button
          @click="calcular"
          class="mt-6 px-4 py-2 bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75"
        >
          Calcular
        </button>
      </div>

      <!-- Componente de resultados -->
      <Resultados
        :mostrarResultados="mostrarResultados"
        :consumoMensualTotal="consumoMensualTotal"
        :panelesSolaresRequeridos="panelesSolaresRequeridos"
        :solarPanelOutput="solarPanelOutput"
        :eficienciaPanel="eficienciaPanel"
        :tipoPanel="tipoPanel"
        :generacionEnergiaAnual="generacionEnergiaAnual"
      />
    </div>
  </div>
</template>

<script>
import InfoPanelSolar from "./InfoPanelSolar.vue";
import Resultados from "./Resultados.vue";
import ElectrodomesticoItem from "./ElectrodomesticoItem.vue";

export default {
  components: {
    InfoPanelSolar,
    Resultados,
    ElectrodomesticoItem,
  },
  data() {
    return {
      electrodomesticos: [
        { nombre: "Televisor", watts: 175, horasPorDia: 0, cantidad: 0 },
        { nombre: "Licuadora", watts: 300, horasPorDia: 0, cantidad: 0 },
        { nombre: "Bombilla", watts: 20, horasPorDia: 0, cantidad: 0 },
        { nombre: "Lavadora", watts: 350, horasPorDia: 0, cantidad: 0 },
        { nombre: "Computador", watts: 200, horasPorDia: 0, cantidad: 0 },
        { nombre: "Brilladora", watts: 500, horasPorDia: 0, cantidad: 0 },
      ],
      solarPanelOutput: 250,
      horasSolDia: 5,
      eficienciaPanel: 15,
      tipoPanel: "monocristalino",
      consumoMensualTotal: 0,
      panelesSolaresRequeridos: 0,
      generacionEnergiaAnual: 0,
      mostrarResultados: false,
    };
  },
  methods: {
    updatePanelInfo(info) {
      this.tipoPanel = info.tipoPanel;
      this.solarPanelOutput = info.solarPanelOutput;
      this.eficienciaPanel = info.eficienciaPanel;
      this.horasSolDia = info.horasSolDia;
    },
    updateElectrodomestico(electrodomestico) {
      const index = this.electrodomesticos.findIndex(
        (e) => e.nombre === electrodomestico.nombre
      );
      if (index !== -1) {
        this.electrodomesticos[index] = electrodomestico;
      }
    },
    calcular() {
      this.consumoMensualTotal = this.electrodomesticos.reduce(
        (total, electrodomestico) => {
          return (
            total +
            (electrodomestico.watts *
              electrodomestico.horasPorDia *
              30 *
              electrodomestico.cantidad) /
              1000
          );
        },
        0
      );

      const consumoDiario = this.consumoMensualTotal / 30;
      const produccionSolarDiaria =
        (this.solarPanelOutput *
          this.horasSolDia *
          (this.eficienciaPanel / 100)) /
        1000;

      this.panelesSolaresRequeridos = Math.ceil(
        consumoDiario / produccionSolarDiaria
      );

      this.generacionEnergiaAnual =
        this.panelesSolaresRequeridos * produccionSolarDiaria * 365;

      this.mostrarResultados = true;
    },
  },
};
</script>
