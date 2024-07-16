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
    ahorroEstimado: Number,
  },
  data() {
    return {
      chart: null,
    };
  },
  mounted() {
    this.createChart();
    window.addEventListener("resize", this.handleResize);
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.handleResize);
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
          labels: Array.from({ length: 12 }, (_, i) => `Año ${i + 1}`),
          datasets: [
            {
              label: "Ahorro Acumulado",
              data: ahorros,
              borderColor: "#4CAF50",
              backgroundColor: "rgba(76, 175, 80, 0.2)",
              tension: 0.1,
            },
          ],
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          aspectRatio: 1,
          plugins: {
            legend: {
              position: "top",
              labels: {
                boxWidth: 10,
                font: {
                  size: 10,
                },
              },
            },
            title: {
              display: true,
              text: "Ahorro Acumulado por Año",
              font: {
                size: 14,
              },
            },
          },
          scales: {
            x: {
              title: {
                display: true,
                text: "Años",
                font: {
                  size: 10,
                },
              },
              ticks: {
                font: {
                  size: 8,
                },
              },
            },
            y: {
              beginAtZero: true,
              title: {
                display: true,
                text: "Ahorro Acumulado (Pesos)",
                font: {
                  size: 10,
                },
              },
              ticks: {
                font: {
                  size: 8,
                },
                callback: function (value) {
                  return value.toLocaleString("es-MX", {
                    style: "currency",
                    currency: "MXN",
                    minimumFractionDigits: 0,
                  });
                },
              },
            },
          },
        },
      });
    },
    calcularAhorros() {
      const ahorroTotal = this.ahorroEstimado;
      const ahorros = [];
      for (let año = 1; año <= 12; año++) {
        ahorros.push(Math.round((ahorroTotal / 12) * año));
      }
      return ahorros;
    },
    handleResize() {
      if (this.chart) {
        this.chart.resize();
      }
    },
  },
  watch: {
    consumoAnualTotal() {
      this.createChart();
    },
    generacionEnergiaAnual() {
      this.createChart();
    },
    ahorroEstimado() {
      this.createChart();
    },
  },
};
</script>
