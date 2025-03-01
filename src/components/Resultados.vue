<template>
  <div v-if="mostrarResultados" class="bg-white shadow-md rounded-lg p-4 sm:p-8 max-w-7xl mx-auto">
    <h2 class="text-2xl sm:text-3xl font-bold text-gray-800 mb-6 text-center border-b pb-4">
      Resultados
    </h2>

    <div class="grid md:grid-cols-2 gap-4 sm:gap-8">
      <!-- Sección de Consumo -->
      <div class="bg-transparent sm:bg-white sm:p-4 sm:rounded-lg sm:shadow-md space-y-4">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 flex items-center">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 mr-2 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
          </svg>
          Consumo Energético
        </h3>
        <div class="space-y-3">
          <div class="flex justify-between items-center p-4 bg-green-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Consumo mensual total:</span>
            <span class="text-green-600 font-bold text-lg">{{ consumoMensualTotal.toFixed(2) }} kWh</span>
          </div>
          <div class="flex justify-between items-center p-4 bg-green-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Consumo diario promedio:</span>
            <span class="text-green-600 font-bold text-lg">{{ consumoDiarioPromedio.toFixed(2) }} kWh</span>
          </div>
          <div class="flex justify-between items-center p-4 bg-green-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Consumo anual total:</span>
            <span class="text-green-600 font-bold text-lg">{{ consumoAnualTotal.toFixed(2) }} kWh</span>
          </div>
          
          <!-- Horas de Consumo Energético -->
          <div class=" pt-8">
            <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 flex items-center">
              <svg class="w-5 h-5 sm:w-6 sm:h-6 mr-2 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              Horas de Consumo Energético
            </h3>
            
            <!-- Consumo Diario -->
            <div class="flex justify-between items-center p-5 bg-green-50 rounded-lg mb-3">
              <div>
                <div class="text-base sm:text-lg text-gray-700 font-medium">Consumo Diario</div>
                <div class="text-xs sm:text-sm text-gray-500 mt-2">{{ horasConsumoDiario.toFixed(1) }} horas</div>
              </div>
              <div class="text-right">
                <div class="text-green-600 font-bold text-xl">{{ consumoDiarioPromedio.toFixed(2) }} kWh</div>
                <div class="text-xs sm:text-sm text-green-600 mt-2">{{ (consumoDiarioPromedio / horasConsumoDiario).toFixed(2) }} kWh/h</div>
              </div>
            </div>
            
            <!-- Consumo Mensual -->
            <div class="flex justify-between items-center p-5 bg-green-50 rounded-lg mb-3">
              <div>
                <div class="text-base sm:text-lg text-gray-700 font-medium">Consumo Mensual</div>
                <div class="text-xs sm:text-sm text-gray-500 mt-2">{{ horasConsumoMensual.toFixed(1) }} horas</div>
              </div>
              <div class="text-right">
                <div class="text-green-600 font-bold text-xl">{{ consumoMensualTotal.toFixed(2) }} kWh</div>
                <div class="text-xs sm:text-sm text-green-600 mt-2">{{ (consumoMensualTotal / horasConsumoMensual).toFixed(2) }} kWh/h</div>
              </div>
            </div>
            
            <!-- Consumo Anual -->
            <div class="flex justify-between items-center p-5 bg-green-50 rounded-lg">
              <div>
                <div class="text-base sm:text-lg text-gray-700 font-medium">Consumo Anual</div>
                <div class="text-xs sm:text-sm text-gray-500 mt-2">{{ horasConsumoAnual.toFixed(1) }} horas</div>
              </div>
              <div class="text-right">
                <div class="text-green-600 font-bold text-xl">{{ consumoAnualTotal.toFixed(2) }} kWh</div>
                <div class="text-xs sm:text-sm text-green-600 mt-2">{{ (consumoAnualTotal / horasConsumoAnual).toFixed(2) }} kWh/h</div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Sección de Panel Solar -->
      <div class="bg-transparent sm:bg-white sm:p-4 sm:rounded-lg sm:shadow-md space-y-4">
        <h3 class="text-lg sm:text-xl font-semibold text-gray-700 mb-4 flex items-center">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 mr-2 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707"/>
          </svg>
          Especificaciones del Sistema Solar
        </h3>
        <div class="space-y-3">
          <div class="flex justify-between items-center p-4 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Paneles necesarios:</span>
            <span class="text-blue-600 font-bold text-lg">{{ panelesSolaresRequeridos }}</span>
          </div>
          <div class="flex justify-between items-center p-4 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Potencia del panel:</span>
            <span class="text-blue-600 font-bold text-lg">{{ solarPanelOutput }} watts</span>
          </div>
          <div class="flex justify-between items-center p-4 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Eficiencia:</span>
            <span class="text-blue-600 font-bold text-lg">{{ eficienciaPanel.toFixed(1) }}%</span>
          </div>
          <div class="flex justify-between items-center p-4 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Tipo de panel:</span>
            <span class="text-blue-600 font-bold text-lg">{{ tipoPanelDisplay }}</span>
          </div>
          <div class="flex justify-between items-center p-4 bg-blue-50 rounded-lg">
            <span class="text-sm sm:text-base text-gray-700">Tamaño de área de paneles:</span>
            <span class="text-blue-600 font-bold text-lg flex items-center">
              {{ areaPaneles.toFixed(2) }} m²
              <span
                ref="tooltipArea"
                class="ml-2 text-white bg-gray-400 hover:bg-gray-500 rounded-full w-5 h-5 flex items-center justify-center cursor-pointer text-xs"
              >?</span>
            </span>
          </div>
          
          <!-- Representación gráfica del área de paneles -->
          <div class="mt-3 p-3 bg-blue-50 rounded-lg">
            <h4 class="text-sm font-medium text-gray-700 mb-2">Visualización del área requerida:</h4>
            <div class="relative w-full h-48 rounded-md overflow-hidden">
              <!-- Representación de los paneles -->
              <div class="absolute inset-0 flex items-center justify-center">
                <div v-if="areaPaneles > 0" 
                     :style="{
                       width: '100%',
                       height: '100%',
                       backgroundColor: panelColor,
                       transition: 'all 0.5s ease'
                     }"
                     class="relative flex items-center justify-center">
                  <div class="absolute inset-0 grid" 
                       :style="{
                         gridTemplateColumns: `repeat(${calcularColumnasOptimas(panelesSolaresRequeridos)}, 1fr)`,
                         gridTemplateRows: `repeat(${calcularFilasOptimas(panelesSolaresRequeridos)}, 1fr)`,
                         gap: '2px',
                         padding: '2px'
                       }">
                    <div v-for="n in panelesSolaresRequeridos" :key="n" 
                         class="bg-white opacity-20 border border-blue-300 relative">
                    </div>
                    
                    <!-- Sistema de acotación para las dimensiones -->
                    <div v-if="panelesSolaresRequeridos > 0" class="absolute inset-0 z-30 pointer-events-none">
                      <!-- Acotación horizontal (ancho) - en la parte superior -->
                      <div class="absolute top-0 left-0 right-0 flex items-start">
                        <div class="w-full flex items-center">
                          <!-- Línea de acotación horizontal -->
                          <div class="h-[1px] bg-white w-full relative">
                            <!-- Líneas verticales en los extremos -->
                            <div class="absolute left-0 h-2 w-[1px] bg-white"></div>
                            <div class="absolute right-0 h-2 w-[1px] bg-white"></div>
                            <!-- Texto de medida - ANCHO TOTAL (columnas * ancho de panel) -->
                            <div class="absolute top-2 left-1/2 transform -translate-x-1/2 text-white text-xs font-bold whitespace-nowrap">
                              {{ (dimensionesPaneles[tipoPanel]?.alto * calcularColumnasOptimas(panelesSolaresRequeridos)).toFixed(1) }}m
                            </div>
                          </div>
                        </div>
                      </div>
                      
                      <!-- Acotación vertical (alto) - en el lateral izquierdo -->
                      <div class="absolute top-0 bottom-0 left-0 flex items-start h-full">
                        <div class="h-full flex flex-col items-center">
                          <!-- Línea de acotación vertical -->
                          <div class="w-[1px] bg-white h-full relative">
                            <!-- Líneas horizontales en los extremos -->
                            <div class="absolute top-0 w-2 h-[1px] bg-white"></div>
                            <div class="absolute bottom-0 w-2 h-[1px] bg-white"></div>
                            <!-- Texto de medida - ALTO TOTAL (filas * alto de panel) -->
                            <div class="absolute left-2 top-1/2 transform -translate-y-1/2 text-white text-xs font-bold whitespace-nowrap" style="writing-mode: vertical-rl; transform: rotate(180deg);">
                              {{ (dimensionesPaneles[tipoPanel]?.ancho * calcularFilasOptimas(panelesSolaresRequeridos)).toFixed(1) }}m
                            </div>
                          </div>
                        </div>
                      </div>
                      
                      <!-- Dimensión del panel individual (solo en el primer panel) -->
                      <div class="absolute top-0 left-0 text-white text-xs font-bold p-1">
                        {{ dimensionesPaneles[tipoPanel]?.ancho.toFixed(1) }}m × {{ dimensionesPaneles[tipoPanel]?.alto.toFixed(1) }}m
                      </div>
                    </div>
                  </div>
                  <div class="text-white text-center font-bold text-sm z-10 p-2">
                    <div class="flex items-center justify-center">
                      {{ areaPaneles.toFixed(1) }} m²
                    </div>
                    <div>{{ panelesSolaresRequeridos }} paneles</div>
                  </div>
                </div>
                <div v-else class="text-gray-400 text-center">
                  Seleccione especificaciones del panel para visualizar el área
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Sección de Costos y Ahorro -->
      <div class="bg-transparent sm:bg-white sm:p-4 sm:rounded-lg sm:shadow-md space-y-4 md:col-span-2">
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
      <div class="bg-transparent sm:bg-white  sm:p-6 w-full overflow-hidden sm:rounded-lg sm:shadow-md">
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
      <div class="bg-transparent sm:bg-white sm:p-6 w-full overflow-hidden sm:rounded-lg sm:shadow-md">
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
    horasDiarias: {
      type: Number,
      default: 0
    }
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
      },
      // Colores para los diferentes tipos de paneles
      coloresPaneles: {
        monocristalino: "#1a365d", // Azul oscuro
        polycristalino: "#2563eb", // Azul medio
        peliculaDelgada: "#60a5fa"  // Azul claro
      },
      // Dimensiones típicas de paneles solares según tipo (en metros)
      dimensionesPaneles: {
        monocristalino: { ancho: 1.0, alto: 1.7 }, // Típicamente más eficientes, menor área
        polycristalino: { ancho: 1.0, alto: 1.65 }, // Ligeramente más grandes que los monocristalinos
        peliculaDelgada: { ancho: 1.2, alto: 2.0 }  // Generalmente más grandes por su menor eficiencia
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
      
      if (this.$refs.tooltipArea) {
        // Obtener dimensiones del panel
        const dimensiones = this.dimensionesPaneles[this.tipoPanel];
        if (!dimensiones) return;
        
        // Calcular área por panel
        const areaPorPanel = dimensiones.ancho * dimensiones.alto;
        
        // Obtener eficiencia usada
        const eficienciasTipicas = {
          monocristalino: 20,
          polycristalino: 16,
          peliculaDelgada: 11
        };
        let eficienciaUsada = this.eficienciaPanel;
        const eficienciaTipica = eficienciasTipicas[this.tipoPanel] || 15;
        if (eficienciaUsada < 5 || eficienciaUsada > 25) {
          eficienciaUsada = eficienciaTipica;
        }
        
        // Calcular área necesaria por panel
        const areaNecesaria = this.solarPanelOutput / (1000 * (eficienciaUsada/100));
        
        // Factor de ajuste
        let factorAjuste = 1.1;
        if (this.tipoPanel === 'peliculaDelgada') {
          factorAjuste = 1.2;
        }
        
        // Área total
        const areaTotal = this.panelesSolaresRequeridos * areaNecesaria * factorAjuste;
        
        // Crear contenido del tooltip con la fórmula explicada
        const tooltipContent = `
          <div style="text-align: left; font-size: 13px;">
            <p><strong>Fórmula:</strong> Área total = Número de paneles × Área por panel × Factor de ajuste</p>
            <p>Donde:</p>
            <p>- Área por panel = ${areaPorPanel.toFixed(2)} m² (${dimensiones.ancho.toFixed(1)}m × ${dimensiones.alto.toFixed(1)}m)</p>
            <p>- Factor de ajuste = ${factorAjuste.toFixed(1)} (espacio adicional para instalación)</p>
            <p>Resultado: ${this.panelesSolaresRequeridos} × ${areaPorPanel.toFixed(2)} × ${factorAjuste.toFixed(1)} = ${areaTotal.toFixed(1)} m²</p>
          </div>
        `;
        
        tippy(this.$refs.tooltipArea, {
          content: tooltipContent,
          placement: "top",
          arrow: true,
          allowHTML: true,
          interactive: true,
          maxWidth: 350
        });
      }
    },
    updateChartKey() {
      this.chartKey += 1;
      this.lineChartKey += 1;
    },
    calcularColumnasOptimas(paneles) {
      // Calcular el número óptimo de columnas basado en la raíz cuadrada
      // y ajustando para una mejor distribución visual
      if (paneles <= 2) return paneles;
      if (paneles <= 4) return 2;
      if (paneles <= 6) return 3;
      if (paneles <= 9) return 3;
      if (paneles <= 12) return 4;
      if (paneles <= 16) return 4;
      return Math.ceil(Math.sqrt(paneles));
    },
    calcularFilasOptimas(paneles) {
      // Calcular el número de filas necesarias basado en el número de columnas
      const columnas = this.calcularColumnasOptimas(paneles);
      return Math.ceil(paneles / columnas);
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
    panelColor() {
      return this.coloresPaneles[this.tipoPanel] || "#3b82f6"; // Color por defecto si no hay tipo seleccionado
    },
    consumoDiarioPromedio() {
      return this.consumoMensualTotal / 30;
    },
    consumoAnualTotal() {
      return this.consumoMensualTotal * 12;
    },
    // Propiedades computadas para las horas de consumo
    horasConsumoDiario() {
      return this.horasDiarias;
    },
    horasConsumoMensual() {
      return this.horasDiarias * 30;
    },
    horasConsumoAnual() {
      return this.horasDiarias * 365;
    },
    areaPaneles() {
      // Si no hay tipo de panel seleccionado o no hay paneles requeridos, retornar 0
      if (!this.tipoPanel || !this.panelesSolaresRequeridos) {
        return 0;
      }
      
      // Obtener las dimensiones según el tipo de panel
      const dimensiones = this.dimensionesPaneles[this.tipoPanel];
      
      // Si no hay dimensiones para este tipo, usar un valor predeterminado
      if (!dimensiones) {
        return this.panelesSolaresRequeridos * 1.7; // Valor promedio
      }
      
      // Calcular el área de un panel
      const areaPorPanel = dimensiones.ancho * dimensiones.alto;
      
      // Usar la eficiencia real del panel para calcular el área necesaria
      // La fórmula básica es: Área = Potencia / (Irradiancia * Eficiencia)
      // Donde la irradiancia estándar es 1000 W/m²
      
      // Eficiencias típicas por tipo de panel (como referencia)
      const eficienciasTipicas = {
        monocristalino: 20, // 18-22%
        polycristalino: 16, // 15-17%
        peliculaDelgada: 11  // 10-12%
      };
      
      // Usar la eficiencia proporcionada por el usuario, o la típica si está fuera de rango
      let eficienciaUsada = this.eficienciaPanel;
      
      // Verificar si la eficiencia está dentro de rangos razonables para el tipo de panel
      const eficienciaTipica = eficienciasTipicas[this.tipoPanel] || 15;
      if (eficienciaUsada < 5 || eficienciaUsada > 25) {
        eficienciaUsada = eficienciaTipica;
      }
      
      // Calcular el área necesaria para generar la potencia requerida
      // Área (m²) = Potencia (W) / (1000 W/m² * eficiencia/100)
      const areaNecesaria = this.solarPanelOutput / (1000 * (eficienciaUsada/100));
      
      // Ajustar el área según el tipo de panel (factor de forma, espaciado, etc.)
      let factorAjuste = 1.1; // 10% adicional para marcos, espaciado, etc.
      
      if (this.tipoPanel === 'peliculaDelgada') {
        factorAjuste = 1.2; // Película delgada suele necesitar más espacio de instalación
      }
      
      // Calcular el área total
      return this.panelesSolaresRequeridos * areaNecesaria * factorAjuste;
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
