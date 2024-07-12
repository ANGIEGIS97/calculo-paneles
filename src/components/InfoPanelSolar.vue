<template>
  <div class="mt-6 bg-gray-100 p-4 rounded-md shadow">
    <h3 class="text-lg font-medium text-gray-800 mb-2">
      Información del Panel Solar
    </h3>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <!-- Selector para el tipo de panel -->
      <div>
        <label
          for="panel-type"
          class="text-sm font-medium text-gray-700 flex items-center"
        >
          Tipo de panel
          <span
            ref="tooltip1"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 flex items-center justify-center cursor-pointer"
            >?</span
          >
        </label>
        <select
          v-model="tipoPanel"
          id="panel-type"
          class="mt-1 block w-full rounded-md bg-white border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
          @change="actualizarRangosPanel"
        >
          <option value="monocristalino">
            Monocristalino (Alta eficiencia)
          </option>
          <option value="polycristalino">
            Policristalino (Media eficiencia)
          </option>
          <option value="peliculaDelgada">
            Película delgada (Baja eficiencia)
          </option>
        </select>
      </div>
      <!-- Control deslizante para la potencia del panel -->
      <div>
        <label
          for="panel-potencia"
          class="flex text-sm font-medium text-gray-700"
        >
          Potencia del panel ({{ rangoPoder.min }} - {{ rangoPoder.max }} W)
          <span
            ref="tooltip2"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 flex items-center justify-center cursor-pointer"
            >?</span
          >
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
          class="flex text-sm font-medium text-gray-700 items-center"
        >
          Eficiencia del panel ({{ rangoEficiencia.min }}% -
          {{ rangoEficiencia.max }}%)
          <span
            ref="tooltip3"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 flex items-center justify-center cursor-pointer"
            >?</span
          >
          <span
            class="ml-2 w-4 h-4 rounded-full"
            :class="eficienciaColor"
          ></span>
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
      <!-- Selector para las horas de sol por día -->
      <div>
        <label for="horas-sol" class="flex text-sm font-medium text-gray-700">
          Horas de sol por día (efectivas)
          <span
            ref="tooltip4"
            class="ml-2 text-white bg-gray-400 rounded-full w-4 h-4 flex items-center justify-center cursor-pointer"
            >?</span
          >
        </label>
        <select
          v-model="opcionHorasSol"
          id="horas-sol"
          class="mt-1 block w-full rounded-md bg-white border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
          @change="actualizarHorasSol"
        >
          <option value="bogota">Bogotá (4.5 horas)</option>
          <option value="guajira">Guajira (6.5 horas)</option>
          <option value="personalizada">Personalizada</option>
        </select>
        <input
          v-if="opcionHorasSol === 'personalizada'"
          v-model.number="horasSolDia"
          type="number"
          class="mt-2 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
          placeholder="Ingrese las horas de sol por día"
          step="0.1"
        />
        <span v-else class="text-sm text-gray-600 mt-2 block"
          >{{ horasSolDia }} horas</span
        >
      </div>
    </div>
  </div>
</template>

<script>
import tippy from "tippy.js";
import "tippy.js/dist/tippy.css";

export default {
  data() {
    return {
      solarPanelOutput: 250,
      horasSolDia: 4.5,
      eficienciaPanel: 15,
      tipoPanel: "monocristalino",
      rangoEficiencia: { min: 15, max: 22 },
      rangoPoder: { min: 250, max: 500 },
      opcionHorasSol: "bogota",
    };
  },
  computed: {
    eficienciaColor() {
      if (this.eficienciaPanel < 13) {
        return "bg-red-500";
      } else if (this.eficienciaPanel < 17) {
        return "bg-yellow-500";
      } else {
        return "bg-green-500";
      }
    },
  },
  methods: {
    actualizarRangosPanel() {
      switch (this.tipoPanel) {
        case "monocristalino":
          this.rangoEficiencia = { min: 15, max: 22 };
          this.rangoPoder = { min: 250, max: 500 };
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
      this.emitirActualizacion();
    },
    actualizarHorasSol() {
      switch (this.opcionHorasSol) {
        case "bogota":
          this.horasSolDia = 4.5;
          break;
        case "guajira":
          this.horasSolDia = 6.5;
          break;
        case "personalizada":
          // No cambiamos el valor, el usuario lo ingresará
          break;
      }
      this.emitirActualizacion();
    },
    emitirActualizacion() {
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
    tippy(this.$refs.tooltip1, {
      content:
        "Hay tres tipos de paneles: Monocristalino, Policristalino y Película delgada.",
    });
    tippy(this.$refs.tooltip2, {
      content:
        "La potencia del panel se mide en vatios (W) y varía según el tipo de panel.",
    });
    tippy(this.$refs.tooltip3, {
      content:
        "La eficiencia del panel indica el porcentaje de luz solar que se convierte en energía.",
    });
    tippy(this.$refs.tooltip4, {
      content:
        "Son las horas en las que la luz solar es lo suficientemente intensa para ser considerada plenamente efectiva para la generación de energía",
    });
  },
  watch: {
    solarPanelOutput() {
      this.emitirActualizacion();
    },
    eficienciaPanel() {
      this.emitirActualizacion();
    },
    horasSolDia() {
      this.emitirActualizacion();
    },
  },
};
</script>
