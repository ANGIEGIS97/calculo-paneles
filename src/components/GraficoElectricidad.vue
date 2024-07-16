<template>
  <div>
    <canvas ref="chart"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js/auto";

export default {
  props: {
    generacionEnergiaAnual: Number,
    consumoAnualTotal: Number,
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
      const ctx = this.$refs.chart.getContext("2d");
      this.chart = new Chart(ctx, {
        type: "line",
        data: {
          labels: [
            "Enero",
            "Febrero",
            "Marzo",
            "Abril",
            "Mayo",
            "Junio",
            "Julio",
            "Agosto",
            "Septiembre",
            "Octubre",
            "Noviembre",
            "Diciembre",
          ],
          datasets: [
            {
              label: "Generación eléctrica panel",
              data: this.generateMonthlyData(this.generacionEnergiaAnual),
              borderColor: "rgba(75, 192, 192, 1)",
              backgroundColor: "rgba(75, 192, 192, 0.2)",
              tension: 0.4,
              cubicInterpolationMode: "monotone",
            },
            {
              label: "Consumo electrodomésticos",
              data: this.generateMonthlyData(this.consumoAnualTotal),
              borderColor: "rgba(255, 99, 132, 1)",
              backgroundColor: "rgba(255, 99, 132, 0.2)",
              tension: 0.4,
              cubicInterpolationMode: "monotone",
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
              text: "Generación vs Consumo",
              font: {
                size: 14,
              },
            },
          },
          scales: {
            x: {
              ticks: {
                maxRotation: 90,
                minRotation: 90,
                font: {
                  size: 8,
                },
              },
            },
            y: {
              beginAtZero: true,
              title: {
                display: true,
                text: "kWh",
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
          },
        },
      });
    },
    generateMonthlyData(annualValue) {
      // Simula datos mensuales dividiendo el valor anual entre 12 y añadiendo una variación aleatoria
      return Array.from(
        { length: 12 },
        () => annualValue / 12 + (Math.random() - 0.5) * (annualValue / 60)
      );
    },
  },
  watch: {
    generacionEnergiaAnual() {
      this.createChart();
    },
    consumoAnualTotal() {
      this.createChart();
    },
  },
};
</script>
