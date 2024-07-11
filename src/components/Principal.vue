<template>
  <div class="bg-gray-100 min-h-screen py-8">
    <div class="container mx-auto px-4">
      <!-- Ingresar datos de electrodomésticos -->
      <div class="bg-white shadow-md rounded-lg p-6 mb-8">
        <h2 class="text-xl font-semibold mb-4 text-gray-700">
          Ingrese los datos de sus electrodomésticos:
        </h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <!-- Iteración sobre cada electrodoméstico -->
          <div
            v-for="(electrodomestico, index) in electrodomesticos"
            :key="index"
            class="bg-gray-50 p-4 rounded-md shadow"
          >
            <h3 class="text-lg font-medium text-gray-800 mb-2">
              {{ electrodomestico.nombre }}
            </h3>
            <div class="space-y-3">
              <!-- Input para la potencia del electrodoméstico -->
              <div>
                <label
                  :for="`watts-${index}`"
                  class="block text-sm font-medium text-gray-700"
                  >Potencia (watts)</label
                >
                <input
                  v-model.number="electrodomestico.watts"
                  type="number"
                  :id="`watts-${index}`"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
                  :placeholder="`Watts de ${electrodomestico.nombre}`"
                />
              </div>
              <!-- Input para las horas de uso diario -->
              <div>
                <label
                  :for="`hours-${index}`"
                  class="block text-sm font-medium text-gray-700"
                  >Horas de uso diario</label
                >
                <input
                  v-model.number="electrodomestico.hoursPerDay"
                  type="number"
                  :id="`hours-${index}`"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
                  placeholder="Horas por día"
                />
              </div>
            </div>
          </div>
        </div>

        <!-- Componente de información del panel solar -->
        <SolarPanelInfo @update:panel-info="updatePanelInfo" />

        <!-- Botón para iniciar el cálculo -->
        <button
          @click="calcular"
          class="mt-6 px-4 py-2 bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75"
        >
          Calcular
        </button>
      </div>

      <!-- Componente de resultados -->
      <ResultadosPanel
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
import SolarPanelInfo from "./SolarPanelInfo.vue";
import ResultadosPanel from "./ResultadosPanel.vue";

export default {
  components: {
    SolarPanelInfo,
    ResultadosPanel,
  },
  data() {
    return {
      electrodomesticos: [
        { nombre: "Televisor", watts: 0, hoursPerDay: 0 },
        { nombre: "Licuadora", watts: 0, hoursPerDay: 0 },
        { nombre: "Bombilla", watts: 0, hoursPerDay: 0 },
        { nombre: "Lavadora", watts: 0, hoursPerDay: 0 },
        { nombre: "Computador", watts: 0, hoursPerDay: 0 },
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
    calcular() {
      this.consumoMensualTotal = this.electrodomesticos.reduce(
        (total, electrodomestico) => {
          return (
            total +
            (electrodomestico.watts * electrodomestico.hoursPerDay * 30) / 1000
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
