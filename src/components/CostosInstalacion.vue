<template>
  <div class="mt-8 bg-white rounded-lg">
    <h2
      class="text-xl sm:text-2xl font-bold text-gray-800 mb-4 sm:text-left text-center"
    >
      Desglose de Costos de Instalación
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <div class="bg-gray-100 p-4 rounded-lg">
        <h3 class="font-semibold text-base sm:text-lg mb-2 flex items-center">
          Paneles Solares
          <span
            ref="tooltipPaneles"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 inline-flex items-center justify-center cursor-pointer text-xs"
            >?</span
          >
        </h3>
        <p class="text-green-600 font-bold">
          {{ costosPaneles.toLocaleString() }} pesos
          <span class="text-gray-500 text-sm">
            ({{ panelesSolaresRequeridos }} paneles, {{ tipoPanel }} de
            {{ solarPanelOutput }}W)
          </span>
        </p>
      </div>
      <div class="bg-gray-100 p-4 rounded-lg">
        <h3 class="font-semibold text-base sm:text-lg mb-2 flex items-center">
          Inversor
          <span
            ref="tooltipInversor"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 inline-flex items-center justify-center cursor-pointer text-xs"
            >?</span
          >
        </h3>
        <p class="text-green-600 font-bold">2.500.000 pesos</p>
      </div>
      <div class="bg-gray-100 p-4 rounded-lg">
        <h3 class="font-semibold text-base sm:text-lg mb-2 flex items-center">
          Instalación
          <span
            ref="tooltipInstalacion"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 inline-flex items-center justify-center cursor-pointer text-xs"
            >?</span
          >
        </h3>
        <p class="text-green-600 font-bold">
          {{ costosInstalacion.toLocaleString() }} pesos
        </p>
      </div>
      <div class="bg-gray-100 p-4 rounded-lg">
        <h3 class="font-semibold text-base sm:text-lg mb-2 flex items-center">
          Accesorios y Estructura
          <span
            ref="tooltipAccesorios"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 inline-flex items-center justify-center cursor-pointer text-xs"
            >?</span
          >
        </h3>
        <p class="text-green-600 font-bold">1.500.000 pesos</p>
      </div>
    </div>
    <div class="mt-6 bg-blue-100 p-4 rounded-lg">
      <h3 class="font-semibold text-base sm:text-lg mb-2">
        Costo Total de Instalación
      </h3>
      <p class="text-blue-600 font-bold text-base sm:text-lg">
        {{ costoTotal.toLocaleString() }} pesos
      </p>
    </div>
    <div class="mt-4 bg-yellow-100 p-4 rounded-lg">
      <h3 class="font-semibold text-base sm:text-lg mb-2 flex items-center">
        Costo de Mantenimiento Anual
        <span
          ref="tooltipMantenimiento"
          class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 inline-flex items-center justify-center cursor-pointer text-xs"
          >?</span
        >
      </h3>
      <p class="text-yellow-600 font-bold">
        {{ costoMantenimiento.toLocaleString() }} pesos
      </p>
    </div>
  </div>
</template>

<script>
import tippy from "tippy.js";
import "tippy.js/dist/tippy.css";

export default {
  props: {
    panelesSolaresRequeridos: Number,
    solarPanelOutput: Number,
    eficienciaPanel: Number,
    tipoPanel: String,
  },
  data() {
    return {
      costoPanelBase: 300000,
      costoInstalacionBase: 1500000,
      costoInversor: 2500000,
      costoAccesoriosEstructura: 1500000,
    };
  },
  computed: {
    costosPaneles() {
      let costo = this.costoPanelBase;

      // Ajuste por eficiencia
      costo += (this.eficienciaPanel - 15) * 10000;

      // Ajuste por potencia
      costo += (this.solarPanelOutput - 250) * 1000;

      // Ajuste por tipo
      if (this.tipoPanel === "monocristalino") {
        costo += 100000;
      }

      return (
        Math.min(Math.max(costo, 300000), 1200000) *
        this.panelesSolaresRequeridos
      );
    },
    costosInstalacion() {
      return Math.min(
        this.costoInstalacionBase * this.panelesSolaresRequeridos,
        1500000
      );
    },
    costoTotal() {
      return (
        this.costosPaneles +
        this.costoInversor +
        this.costosInstalacion +
        this.costoAccesoriosEstructura
      );
    },
    costoMantenimiento() {
      // 2% de (costo de paneles + inversor + accesorios y estructura)
      return (this.costosPaneles + this.costoInversor) * 0.02;
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.initTooltips();
    });
  },
  methods: {
    initTooltips() {
      tippy(this.$refs.tooltipPaneles, {
        content: `Costo estimado los precios pueden variar.`,
      });
      tippy(this.$refs.tooltipInversor, {
        content:
          "Un inversor convierte la corriente continua (DC) generada por los paneles solares en corriente alterna (AC), utilizada por la mayoría de los dispositivos eléctricos.",
      });
      tippy(this.$refs.tooltipInstalacion, {
        content:
          "El costo de instalación puede variar dependiendo de la complejidad y el tamaño del sistema.",
      });
      tippy(this.$refs.tooltipAccesorios, {
        content:
          "Los accesorios y la estructura de montaje pueden costar entre 1.500.000 y 3.000.000 pesos.",
      });
      tippy(this.$refs.tooltipMantenimiento, {
        content:
          "El costo de mantenimiento anual se estima en un 2% del costo total de los paneles y el inversor",
      });
    },
  },
};
</script>
