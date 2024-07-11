<template>
    <div v-if="mostrarResultados" class="bg-white shadow-md rounded-lg p-6">
      <h2 class="text-2xl font-bold text-gray-800 mb-4">Resultados</h2>
      <div class="space-y-4">
        <p class="text-lg">
          <span class="font-medium">Consumo mensual total:</span>
          <span class="text-green-600 font-bold"
            >{{ consumoMensualTotal.toFixed(2) }} kWh</span
          >
        </p>
        <p class="text-lg">
          <span class="font-medium">Paneles solares necesarios:</span>
          <span class="text-purple-600 font-bold">{{
            panelesSolaresRequeridos
          }}</span>
        </p>
        <p class="text-lg">
          <span class="font-medium">Potencia del panel:</span>
          <span class="text-blue-600 font-bold"
            >{{ solarPanelOutput }} watts</span
          >
        </p>
        <p class="text-lg">
          <span class="font-medium">Eficiencia del panel:</span>
          <span class="text-blue-600 font-bold"
            >{{ eficienciaPanel.toFixed(1) }}%</span
          >
        </p>
        <p class="text-lg">
          <span class="font-medium">Tipo de panel:</span>
          <span class="text-blue-600 font-bold">{{ tipoPanelDisplay }}</span>
        </p>
        <p class="text-lg">
          <span class="font-medium">Energía generada por año:</span>
          <span class="text-blue-600 font-bold"
            >{{ generacionEnergiaAnual.toFixed(2) }} kWh</span
          >
        </p>
      </div>
    </div>
  </template>
  
  <script>
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
    computed: {
      tipoPanelDisplay() {
        const types = {
          monocristalino: "Monocristalino",
          polycristalino: "Policristalino",
          peliculaDelgada: "Película delgada",
        };
        return types[this.tipoPanel];
      },
    },
  };
  </script>
  