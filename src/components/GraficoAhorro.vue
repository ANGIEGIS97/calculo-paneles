<template>
  <div>
    <canvas ref="chart"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js/auto";

export default {
  props: {
    consumoAnualTotal: Number,
    generacionEnergiaAnual: Number,
  },
  data() {
    return {
      chart: null,
    };
  },
  mounted() {
    this.createChart();
  },
  methods: {
    createChart() {
      if (this.chart) {
        this.chart.destroy();
      }

      const ahorros = this.calcularAhorros();

      const ctx = this.$refs.chart.getContext("2d");
      this.chart = new Chart(ctx, {
        type: "line",
        data: {
          labels: Array.from({ length: 12 }, (_, i) => i + 1),
          datasets: [
            {
              label: "Ahorro Acumulado",
              data: ahorros,
              borderColor: "rgb(75, 192, 192)",
              tension: 0.1,
            },
          ],
        },
        options: {
          responsive: true,
          scales: {
            x: {
              title: {
                display: true,
                text: "Años",
              },
            },
            y: {
              beginAtZero: true,
              title: {
                display: true,
                text: "Ahorro Acumulado (Pesos)",
              },
            },
          },
        },
      });
    },
    calcularAhorros() {
      const costoElectricidadAnual = this.consumoAnualTotal * 867.8;
      const ahorroAnual = Math.min(
        costoElectricidadAnual,
        this.generacionEnergiaAnual * 867.8
      );
      const inflacionAnual = 0.04; // 4% de inflación anual
      let ahorroTotal = 0;
      const ahorros = [];

      for (let año = 1; año <= 12; año++) {
        ahorroTotal += ahorroAnual * Math.pow(1 + inflacionAnual, año - 1);
        ahorros.push(Math.round(ahorroTotal));
      }

      return ahorros;
    },
  },
  watch: {
    consumoAnualTotal() {
      this.createChart();
    },
    generacionEnergiaAnual() {
      this.createChart();
    },
  },
};
</script>
