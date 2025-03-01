<template>
  <div class="bg-gray-100 p-4 rounded-md shadow">
    <div class="flex items-center justify-between">
      <div class="flex items-center">
        <div class="w-8 h-8 mr-3 text-blue-500">
          <TvIcon v-if="electrodomestico.nombre === 'Televisor'" class="w-full h-full" />
          <Cog6ToothIcon v-if="electrodomestico.nombre === 'Licuadora'" class="w-full h-full" />
          <LightBulbIcon v-if="electrodomestico.nombre === 'Bombilla'" class="w-full h-full" />
          <WrenchScrewdriverIcon v-if="electrodomestico.nombre === 'Lavadora'" class="w-full h-full" />
          <ComputerDesktopIcon v-if="electrodomestico.nombre === 'Computador'" class="w-full h-full" />
          <SparklesIcon v-if="electrodomestico.nombre === 'Brilladora'" class="w-full h-full" />
          <PlusCircleIcon v-if="electrodomestico.nombre === 'Personalizado'" class="w-full h-full" />
        </div>
        <h3 class="text-lg font-medium text-gray-800">
          {{ electrodomestico.nombre }}
        </h3>
        <!-- Badge cantidad -->
        <div
          :class="[
            'ml-2 text-white text-xs font-bold rounded-full w-6 h-6 flex items-center justify-center cursor-pointer',
            electrodomestico.cantidad > 0 ? 'bg-blue-500' : 'bg-gray-500',
          ]"
          @click="incrementarCantidad"
        >
          {{ Math.max(electrodomestico.cantidad || 0, 0) }}
        </div>
        <!-- Badge horas de uso diario -->
        <div
          :class="[
            'ml-2 text-white text-xs font-bold rounded-full w-6 h-6 flex items-center justify-center cursor-pointer',
            electrodomestico.cantidad > 0 && electrodomestico.horasPorDia > 0
              ? 'bg-blue-500'
              : 'bg-gray-500',
          ]"
          @click="incrementarHoras"
        >
          {{ electrodomestico.horasPorDia || 0 }}h
        </div>
        <!-- Badge watts -->
        <div
          class="ml-2 bg-blue-500 text-white text-xs font-bold rounded-full px-2 py-1 flex items-center justify-center"
        >
          {{ electrodomestico.watts || 0 }}W
        </div>
      </div>
      <span
        class="text-gray-600 md:hidden cursor-pointer"
        @click="mostrarDetalles"
      >
        {{ estaExpandido ? "▲" : "▼" }}
      </span>
    </div>
    <div :class="{ 'hidden md:block': !estaExpandido }" class="space-y-3 mt-4">
      <div>
        <label
          :for="`cantidad-${index}`"
          class="block text-sm font-medium text-gray-700"
        >
          Cantidad: {{ electrodomestico.cantidad }}
        </label>
        <input
          v-model.number="electrodomestico.cantidad"
          type="range"
          :id="`cantidad-${index}`"
          class="mt-1 block w-full"
          min="0"
          max="10"
          step="1"
          @input="actualizarElectrodomestico"
        />
        <div class="flex justify-between text-xs text-gray-500">
          <span>0</span>
          <span>5</span>
          <span>10</span>
        </div>
      </div>
      <div>
        <label
          :for="`horas-${index}`"
          class="block text-sm font-medium text-gray-700"
        >
          Horas de uso diario: {{ electrodomestico.horasPorDia }}
        </label>
        <input
          v-model.number="electrodomestico.horasPorDia"
          type="range"
          :id="`horas-${index}`"
          class="mt-1 block w-full"
          min="0"
          max="24"
          step="1"
          @input="actualizarElectrodomestico"
        />
        <div class="flex justify-between text-xs text-gray-500">
          <span>0h</span>
          <span>12h</span>
          <span>24h</span>
        </div>
      </div>
      <div>
        <label
          :for="`watts-${index}`"
          class="block text-sm font-medium text-gray-700"
        >
          Potencia (watts)
        </label>
        <input
          v-model.number="electrodomestico.watts"
          type="number"
          :id="`watts-${index}`"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
          :placeholder="`Watts de ${electrodomestico.nombre}`"
          min="0"
          max="1500"
          @input="actualizarElectrodomestico"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { 
  TvIcon, 
  Cog6ToothIcon, 
  LightBulbIcon, 
  WrenchScrewdriverIcon,
  ComputerDesktopIcon,
  SparklesIcon,
  PlusCircleIcon
} from '@heroicons/vue/24/outline'

export default {
  components: {
    TvIcon,
    Cog6ToothIcon,
    LightBulbIcon,
    WrenchScrewdriverIcon,
    ComputerDesktopIcon,
    SparklesIcon,
    PlusCircleIcon
  },
  props: {
    electrodomestico: Object,
    index: Number,
  },
  data() {
    return {
      estaExpandido: false,
    };
  },
  methods: {
    actualizarElectrodomestico() {
      this.$emit("update", this.electrodomestico);
    },
    mostrarDetalles() {
      this.estaExpandido = !this.estaExpandido;
    },
    incrementarCantidad() {
      this.electrodomestico.cantidad = Math.min(
        (this.electrodomestico.cantidad || 0) + 1,
        10
      );
      this.actualizarElectrodomestico();
    },
    incrementarHoras() {
      this.electrodomestico.horasPorDia = Math.min(
        (this.electrodomestico.horasPorDia || 0) + 1,
        24
      );
      this.actualizarElectrodomestico();
    },
  },
};
</script>
