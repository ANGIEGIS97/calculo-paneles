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
            <span class="text-gray-600">
              (${{ (consumoMensualTotal * 867.8).toFixed(2) }} pesos)
            </span>
          </td>
        </tr>
        <tr class="border-b bg-green-50">
          <td class="py-2 font-medium">Consumo diario promedio:</td>
          <td class="py-2">
            <span class="text-green-600 font-bold">
              {{ consumoDiarioPromedio.toFixed(2) }} kWh
            </span>
            <span class="text-gray-600">
              (${{ (consumoDiarioPromedio * 867.8).toFixed(2) }} pesos)
            </span>
          </td>
        </tr>
        <tr class="border-b bg-green-50">
          <td class="py-2 font-medium">Consumo anual total:</td>
          <td class="py-2">
            <span class="text-green-600 font-bold">
              {{ consumoAnualTotal.toFixed(2) }} kWh
            </span>
            <span class="text-gray-600">
              (${{ (consumoAnualTotal * 867.8).toFixed(2) }} pesos)
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
          <td class="py-2 font-medium">Ahorro estimado anual:</td>
          <td class="py-2">
            <span class="text-yellow-600 font-bold">
              ${{ ahorroEstimado.anual.toLocaleString() }} pesos
            </span>
          </td>
        </tr>
        <tr class="border-b bg-yellow-50">
          <td class="py-2 font-medium flex">
            Ahorro estimado (25 años):
            <span
              ref="tooltip1"
              class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 flex items-center justify-center cursor-pointer"
              >?</span
            >
          </td>
          <td class="py-2">
            <span class="text-yellow-600 font-bold">
              ${{ ahorroEstimado.total.toLocaleString() }} pesos
            </span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import tippy from "tippy.js";
import "tippy.js/dist/tippy.css";

export default {
  props: {
    mostrarResultados: Boolean,
    consumoMensualTotal: Number,
    panelesSolaresRequeridos: Number,
    solarPanelOutput: Number,
    eficienciaPanel: Number,
    tipoPanel: String,
    generacionEnergiaAnual: Number,
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
            "El calculo del ahorro considera *Costo anual de electricidad. *Energia generada anualmente por el sistema solar. *Inflacion anual estimada del 4% en el costo de la electricidad. *El ahorro se acumula año tras año considerando la Inflación. *25 años vida util panel solar",
          placement: "top",
          arrow: true,
        });
      }
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
      const ahorroAnualBase = Math.min(
        costoElectricidadAnual,
        this.generacionEnergiaAnual * 867.8
      );
      const inflacionAnual = 0.04; // 4% de inflación anual
      let ahorroTotal = 0;

      for (let año = 1; año <= 25; año++) {
        ahorroTotal += ahorroAnualBase * Math.pow(1 + inflacionAnual, año - 1);
      }

      const ahorroAnualPromedio = ahorroTotal / 25;

      return {
        anual: Math.round(ahorroAnualPromedio),
        total: Math.round(ahorroTotal),
      };
    },
  },
};
</script>
