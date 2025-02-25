<template>
  <div v-if="mostrarResultados" class="bg-white shadow-md rounded-lg p-4 sm:p-8 max-w-7xl mx-auto">
    <h2 class="text-2xl sm:text-3xl font-bold text-gray-800 mb-6 text-center border-b pb-4">
      Resultados
    </h2>

    <div class="grid md:grid-cols-2 gap-4 sm:gap-8">
      <!-- Sección de Consumo -->
      <div class="bg-transparent sm:bg-white p-4 sm:rounded-lg sm:shadow-md space-y-4">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 flex items-center">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 mr-2 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
          </svg>
          Consumo Energético
        </h3>
        <div class="space-y-3">
          <div class="flex justify-between items-center p-3 bg-green-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Consumo mensual total:</span>
            <span class="text-green-600 font-bold">{{ consumoMensualTotal.toFixed(2) }} kWh</span>
          </div>
          <div class="flex justify-between items-center p-3 bg-green-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Consumo diario promedio:</span>
            <span class="text-green-600 font-bold">{{ consumoDiarioPromedio.toFixed(2) }} kWh</span>
          </div>
          <div class="flex justify-between items-center p-3 bg-green-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Consumo anual total:</span>
            <span class="text-green-600 font-bold">{{ consumoAnualTotal.toFixed(2) }} kWh</span>
          </div>
        </div>
      </div>

      <!-- Sección de Panel Solar -->
      <div class="bg-transparent sm:bg-white p-4 sm:rounded-lg sm:shadow-md space-y-4">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 flex items-center">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 mr-2 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707"/>
          </svg>
          Especificaciones del Sistema Solar
        </h3>
        <div class="space-y-3">
          <div class="flex justify-between items-center p-3 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Paneles necesarios:</span>
            <span class="text-blue-600 font-bold">{{ panelesSolaresRequeridos }}</span>
          </div>
          <div class="flex justify-between items-center p-3 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Potencia del panel:</span>
            <span class="text-blue-600 font-bold">{{ solarPanelOutput }} watts</span>
          </div>
          <div class="flex justify-between items-center p-3 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Eficiencia:</span>
            <span class="text-blue-600 font-bold">{{ eficienciaPanel.toFixed(1) }}%</span>
          </div>
          <div class="flex justify-between items-center p-3 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Tipo de panel:</span>
            <span class="text-blue-600 font-bold">{{ tipoPanelDisplay }}</span>
          </div>
        </div>
      </div>

      <!-- Sección de Costos y Ahorro -->
      <div class="bg-transparent sm:bg-white p-4 sm:rounded-lg sm:shadow-md space-y-4 md:col-span-2">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 flex items-center">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 mr-2 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
          </svg>
          Información Económica
        </h3>
        <div class="grid md:grid-cols-2 gap-4">
          <div class="space-y-3">
            <div class="p-4 bg-yellow-50 rounded-lg">
              <label class="block text-sm font-medium text-gray-700 mb-3">Seleccione su estrato:</label>
              <div class="grid grid-cols-3 gap-2">
                <button
                  v-for="(rate, estrato) in estratosRates"
                  :key="estrato"
                  @click="selectedEstrato = estrato"
                  :class="[
                    'py-2 px-3 rounded-lg text-sm font-medium transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-300',
                    selectedEstrato === estrato
                      ? 'bg-yellow-500 text-white shadow-md transform scale-105'
                      : 'bg-white text-gray-700 hover:bg-yellow-100 border border-gray-200'
                  ]"
                >
                  Estrato {{ estrato }}
                </button>
              </div>
            </div>
            <div class="p-4 bg-yellow-50 rounded-lg">
              <div class="text-sm text-gray-700 mb-2">Tarifa por kWh:</div>
              <div class="text-xl sm:text-2xl font-bold text-yellow-600">
                ${{ estratosRates[selectedEstrato].toLocaleString() }}
              </div>
              <div class="text-xs text-gray-500">pesos colombianos</div>
            </div>
          </div>
          <div class="p-4 bg-yellow-50 rounded-lg flex flex-col justify-center relative">
            <div class="text-sm text-gray-700 mb-2 flex items-center">
              Ahorro estimado (12 años)
              <span
                ref="tooltip1"
                class="ml-2 text-white bg-gray-400 hover:bg-gray-500 rounded-full w-5 h-5 flex items-center justify-center cursor-pointer"
              >?</span>
            </div>
            <div class="text-xl sm:text-3xl font-bold text-yellow-600">
              ${{ ahorroEstimado.toLocaleString() }}
            </div>
            <div class="text-xs text-gray-500">pesos colombianos</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Sección de Gráficos -->
    <div class="mt-6 sm:mt-8 grid md:grid-cols-2 gap-4 sm:gap-8">
      <div class="bg-transparent sm:bg-white p-3 sm:p-6 w-full overflow-hidden sm:rounded-lg sm:shadow-md">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 text-center">
          Comparación de Energía Anual
        </h3>
        <div class="w-full h-[250px] sm:h-[300px] min-w-0">
          <GraficoElectricidad
            :key="chartKey"
            :generacionEnergiaAnual="generacionEnergiaAnual"
            :consumoAnualTotal="consumoAnualTotal"
          />
        </div>
      </div>
      <div class="bg-transparent sm:bg-white p-3 sm:p-6 w-full overflow-hidden sm:rounded-lg sm:shadow-md">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 text-center">
          Proyección de Ahorro
        </h3>
        <div class="w-full h-[250px] sm:h-[300px] min-w-0">
          <GraficoAhorro
            :key="lineChartKey"
            :consumoAnualTotal="consumoAnualTotal"
            :generacionEnergiaAnual="generacionEnergiaAnual"
            :ahorroEstimado="ahorroEstimado"
          />
        </div>
      </div>
    </div>

    <!-- Sección de Costos de Instalación -->
    <div class="mt-6 sm:mt-8">
      <CostosInstalacion
        :panelesSolaresRequeridos="panelesSolaresRequeridos"
        :solarPanelOutput="solarPanelOutput"
        :eficienciaPanel="eficienciaPanel"
        :tipoPanel="tipoPanel"
      />
    </div>
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
      selectedEstrato: "3",
      estratosRates: {
        "1": 650.5,  // Tarifa aproximada estrato 1
        "2": 813.1,  // Tarifa aproximada estrato 2
        "3": 867.8,  // Tarifa actual estrato 3
        "4": 1021.5, // Tarifa aproximada estrato 4
        "5": 1225.8, // Tarifa aproximada estrato 5
        "6": 1470.9, // Tarifa aproximada estrato 6 (20% más que estrato 5)
      }
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
    selectedEstrato() {
      this.updateChartKey();
    }
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
      const tarifaKwh = this.estratosRates[this.selectedEstrato];
      const generacionMensual = this.generacionEnergiaAnual / 12;
      let ahorroTotal = 0;
      let inflacionAcumulada = 1;
      const inflacionAnual = 0.04; // 4% de inflación fija anual

      for (let año = 1; año <= 12; año++) {
        // Calculamos el ahorro mensual para este año
        const ahorroMensual = Math.min(
          generacionMensual * tarifaKwh * inflacionAcumulada,
          this.consumoMensualTotal * tarifaKwh * inflacionAcumulada
        );
        
        // Multiplicamos por 12 meses para obtener el ahorro anual
        const ahorroAnual = ahorroMensual * 12;
        
        // Agregamos el ahorro de este año al total
        ahorroTotal += ahorroAnual;
        
        // Actualizamos la inflación acumulada para el siguiente año
        inflacionAcumulada *= (1 + inflacionAnual);
      }

      return Math.round(ahorroTotal);
    },
  },
};
</script>
