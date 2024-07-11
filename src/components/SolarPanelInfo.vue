<template>
  <div class="mt-6 bg-gray-50 p-4 rounded-md shadow">
    <h3 class="text-lg font-medium text-gray-800 mb-2">
      Información del Panel Solar
    </h3>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <!-- Selector para el tipo de panel -->
      <div>
        <label for="panel-type" class="block text-sm font-medium text-gray-700"
          >Tipo de panel</label
        >
        <select
          v-model="tipoPanel"
          id="panel-type"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
          @change="actualizarRangosPanel"
        >
          <option value="monocristalino">Monocristalino</option>
          <option value="polycristalino">Policristalino</option>
          <option value="peliculaDelgada">Película delgada</option>
        </select>
      </div>
      <!-- Control deslizante para la potencia del panel -->
      <div>
        <label
          for="panel-potencia"
          class="block text-sm font-medium text-gray-700"
        >
          Potencia del panel ({{ rangoPoder.min }} - {{ rangoPoder.max }} W)
        </label>
        <input
          v-model.number="solarPanelOutput"
          type="range"
          id="panel-potencia"
          :min="rangoPoder.min"
          :max="rangoPoder.max"
          step="1"
          class="mt-1 block w-full"
        />
        <span class="text-sm text-gray-600">{{ solarPanelOutput }} W</span>
      </div>
      <!-- Control deslizante para la eficiencia del panel -->
      <div>
        <label
          for="panel-eficiencia"
          class="block text-sm font-medium text-gray-700"
        >
          Eficiencia del panel ({{ rangoEficiencia.min }}% -
          {{ rangoEficiencia.max }}%)
        </label>
        <input
          v-model.number="eficienciaPanel"
          type="range"
          id="panel-eficiencia"
          :min="rangoEficiencia.min"
          :max="rangoEficiencia.max"
          step="0.1"
          class="mt-1 block w-full"
        />
        <span class="text-sm text-gray-600"
          >{{ eficienciaPanel.toFixed(1) }}%</span
        >
      </div>
      <!-- Input para las horas de sol por día -->
      <div>
        <label for="horas-sol" class="block text-sm font-medium text-gray-700"
          >Horas de sol por día</label
        >
        <input
          v-model.number="horasSolDia"
          type="number"
          id="horas-sol"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
          placeholder="Horas de sol por día"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      solarPanelOutput: 250,
      horasSolDia: 5,
      eficienciaPanel: 15,
      tipoPanel: "monocristalino",
      rangoEficiencia: { min: 15, max: 22 },
      rangoPoder: { min: 250, max: 550 },
    };
  },
  methods: {
    actualizarRangosPanel() {
      switch (this.tipoPanel) {
        case "monocristalino":
          this.rangoEficiencia = { min: 15, max: 22 };
          this.rangoPoder = { min: 250, max: 550 };
          break;
        case "polycristalino":
          this.rangoEficiencia = { min: 13, max: 17 };
          this.rangoPoder = { min: 250, max: 350 };
          break;
        case "peliculaDelgada":
          this.rangoEficiencia = { min: 10, max: 12 };
          this.rangoPoder = { min: 100, max: 300 };
          break;
      }
      this.eficienciaPanel =
        (this.rangoEficiencia.min + this.rangoEficiencia.max) / 2;
      this.solarPanelOutput = Math.round(
        (this.rangoPoder.min + this.rangoPoder.max) / 2
      );
      this.$emit("update:panel-info", {
        tipoPanel: this.tipoPanel,
        solarPanelOutput: this.solarPanelOutput,
        eficienciaPanel: this.eficienciaPanel,
        horasSolDia: this.horasSolDia,
      });
    },
  },
  mounted() {
    this.actualizarRangosPanel();
  },
  watch: {
    solarPanelOutput() {
      this.$emit("update:panel-info", {
        tipoPanel: this.tipoPanel,
        solarPanelOutput: this.solarPanelOutput,
        eficienciaPanel: this.eficienciaPanel,
        horasSolDia: this.horasSolDia,
      });
    },
    eficienciaPanel() {
      this.$emit("update:panel-info", {
        tipoPanel: this.tipoPanel,
        solarPanelOutput: this.solarPanelOutput,
        eficienciaPanel: this.eficienciaPanel,
        horasSolDia: this.horasSolDia,
      });
    },
    horasSolDia() {
      this.$emit("update:panel-info", {
        tipoPanel: this.tipoPanel,
        solarPanelOutput: this.solarPanelOutput,
        eficienciaPanel: this.eficienciaPanel,
        horasSolDia: this.horasSolDia,
      });
    },
  },
};
</script>
