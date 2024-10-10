<template>
  <div v-if="mostrarResultados" class="bg-white shadow-md rounded-lg p-6">
    <h2 class="text-2xl font-bold text-gray-800 mb-4 sm:text-left text-center">
      Resultados
    </h2>
    <table class="w-full border-collapse">
      <tbody>
        <tr class="border-b bg-green-50">
          <td class="py-2 font-medium">Consumo mensual total:</td>
          <td class="py-2">
            <span class="text-green-600 font-bold">
              {{ consumoMensualTotal.toFixed(2) }} kWh
            </span>
          </td>
        </tr>
        <tr class="border-b bg-green-50">
          <td class="py-2 font-medium">Consumo diario promedio:</td>
          <td class="py-2">
            <span class="text-green-600 font-bold">
              {{ consumoDiarioPromedio.toFixed(2) }} kWh
            </span>
          </td>
        </tr>
        <tr class="border-b bg-green-50">
          <td class="py-2 font-medium">Consumo anual total:</td>
          <td class="py-2">
            <span class="text-green-600 font-bold">
              {{ consumoAnualTotal.toFixed(2) }} kWh
            </span>
          </td>
        </tr>
        <tr class="border-b bg-purple-50">
          <td class="py-2 font-medium">Paneles solares necesarios:</td>
          <td class="py-2">
            <span class="text-purple-600 font-bold">
              {{ panelesSolaresRequeridos }}
            </span>
          </td>
        </tr>
        <tr class="border-b bg-blue-50">
          <td class="py-2 font-medium">Potencia del panel:</td>
          <td class="py-2">
            <span class="text-blue-600 font-bold">
              {{ solarPanelOutput }} watts
            </span>
          </td>
        </tr>
        <tr class="border-b bg-blue-50">
          <td class="py-2 font-medium">Eficiencia del panel:</td>
          <td class="py-2">
            <span class="text-blue-600 font-bold">
              {{ eficienciaPanel.toFixed(1) }}%
            </span>
          </td>
        </tr>
        <tr class="border-b bg-blue-50">
          <td class="py-2 font-medium">Tipo de panel:</td>
          <td class="py-2">
            <span class="text-blue-600 font-bold">{{ tipoPanelDisplay }}</span>
          </td>
        </tr>
        <tr class="border-b bg-blue-50">
          <td class="py-2 font-medium">Energía generada por año:</td>
          <td class="py-2">
            <span class="text-blue-600 font-bold">
              {{ generacionEnergiaAnual.toFixed(2) }} kWh
            </span>
          </td>
        </tr>
        <tr class="border-b bg-yellow-50">
          <td class="py-2 font-medium flex items-center">
            Ahorro estimado (12 años):
            <span
              ref="tooltip1"
              class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 flex items-center justify-center cursor-pointer flex-shrink-0"
              >?</span
            >
          </td>
          <td class="py-2">
            <span class="text-yellow-600 font-bold">
              ${{ ahorroEstimado.toLocaleString() }} pesos
            </span>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="mt-6 flex flex-col lg:flex-row gap-6">
      <div class="w-full lg:w-1/2">
        <h3
          class="text-xl font-bold text-gray-800 mb-4 text-center sm:text-left"
        >
          Comparación de Energía Anual
        </h3>
        <GraficoElectricidad
          :key="chartKey"
          :generacionEnergiaAnual="generacionEnergiaAnual"
          :consumoAnualTotal="consumoAnualTotal"
        />
      </div>
      <div class="w-full lg:w-1/2">
        <h3
          class="text-xl font-bold text-gray-800 mb-4 text-center sm:text-left"
        >
          Proyección de Ahorro
        </h3>
        <GraficoAhorro
          :key="lineChartKey"
          :consumoAnualTotal="consumoAnualTotal"
          :generacionEnergiaAnual="generacionEnergiaAnual"
          :ahorroEstimado="ahorroEstimado"
        />
      </div>
    </div>

    <CostosInstalacion
      :panelesSolaresRequeridos="panelesSolaresRequeridos"
      :solarPanelOutput="solarPanelOutput"
      :eficienciaPanel="eficienciaPanel"
      :tipoPanel="tipoPanel"
    />
  </div>
</template>

<script>
import tippy from "tippy.js";
import "tippy.js/dist/tippy.css";
import GraficoElectricidad from "./GraficoElectricidad.vue";
import GraficoAhorro from "./GraficoAhorro.vue";
import CostosInstalacion from "./CostosInstalacion.vue";

export default {
  components: {
    GraficoElectricidad,
    GraficoAhorro,
    CostosInstalacion,
  },
  props: {
    mostrarResultados: Boolean,
    consumoMensualTotal: Number,
    panelesSolaresRequeridos: Number,
    solarPanelOutput: Number,
    eficienciaPanel: Number,
    tipoPanel: String,
    generacionEnergiaAnual: Number,
  },
  data() {
    return {
      chartKey: 0,
      lineChartKey: 0,
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.initTooltip();
    });
  },
  updated() {
    this.initTooltip();
  },
  methods: {
    initTooltip() {
      if (this.$refs.tooltip1) {
        tippy(this.$refs.tooltip1, {
          content:
            "El calculo del ahorro considera *Costo anual de electricidad. *Energia generada anualmente por el sistema solar. *Inflacion anual estimada del 4% en el costo de la electricidad. *El ahorro se acumula año tras año considerando la Inflación. *12 años vida util panel solar",
          placement: "top",
          arrow: true,
        });
      }
    },
    updateChartKey() {
      this.chartKey += 1;
      this.lineChartKey += 1;
    },
  },
  watch: {
    generacionEnergiaAnual() {
      this.updateChartKey();
    },
    consumoAnualTotal() {
      this.updateChartKey();
    },
  },
  computed: {
    tipoPanelDisplay() {
      const types = {
        monocristalino: "Monocristalino",
        polycristalino: "Policristalino",
        peliculaDelgada: "Película delgada",
      };
      return types[this.tipoPanel];
    },
    consumoDiarioPromedio() {
      return this.consumoMensualTotal / 30;
    },
    consumoAnualTotal() {
      return this.consumoMensualTotal * 12;
    },
    ahorroEstimado() {
      const costoElectricidadAnual = this.consumoAnualTotal * 867.8;
      const ahorroAnual = Math.min(
        costoElectricidadAnual,
        this.generacionEnergiaAnual * 867.8
      );

      function inflacionAleatoria() {
        const min = 0.02; // 2% de inflación anual
        const max = 0.1; // 10% de inflación anual
        return Math.random() * (max - min) + min;
      }

      let ahorroTotal = 0;

      for (let año = 1; año <= 12; año++) {
        const inflacionAnual = inflacionAleatoria();
        ahorroTotal += ahorroAnual * Math.pow(1 + inflacionAnual, año - 1);
      }

      return Math.round(ahorroTotal);
    },
  },
};
</script>
