<template>
  <div class="chart-wrapper">
    <LineChart
      :chart-options="chartOptions"
      :chart-data="chartData"
      :chart-id="'chart-graph'"
      :dataset-id-key="'chart-graph__data'"
    />
  </div>
</template>

<script>
import { Line as LineChart } from "vue-chartjs/legacy";

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  CategoryScale,
  PointElement,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  CategoryScale,
  PointElement
);

class Point {
  constructor(min, max) {
    this.value = Math.floor(Math.random() * (max - min + 1)) + min;
  }
}

export default {
  name: "Chart",
  components: {
    LineChart,
  },
  props: {
    average: {
      type: Number,
    },
    deviation: {
      type: Number,
    },
    frequency: {
      type: Number,
      default: 2,
    },
  },
  data() {
    return {
      timeout: null,
      LIMIT: 500,
      chartData: {
        labels: [],
        datasets: [
          {
            backgroundColor: "#f87979",
            data: [],
          },
        ],
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        barPercentage: 1,
        categoryPercentage: 1,
        animation: {
          easing: "linear",
          duration: 0,
        },
        plugins: {
          legend: {
            display: false,
          },
        },
      },
    };
  },
  created() {
    this.drawPoint();
  },
  watch: {
    frequency() {
      this.drawPoint();
    },
    deviation() {
      this.drawPoint();
    },
    average() {
      this.drawPoint();
    },
  },
  computed: {
    maxValue() {
      return this.average + this.deviation;
    },
    minValue() {
      return this.average - this.deviation;
    },
  },
  methods: {
    drawPoint() {
      if (this.frequency > 0 && this.deviation > 0) {
        clearTimeout(this.timeout);
        this.chartData.datasets[0].data.length <= this.LIMIT
          ? this.chartData.labels.push("")
          : this.chartData.datasets[0].data.splice(0, 1);

        const point = new Point(this.minValue, this.maxValue);
        this.chartData.datasets[0].data.push(point.value);

        this.timeout = setTimeout(() => {
          this.drawPoint();
        }, 1000 / this.frequency);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.chart-wrapper {
  background: #fff;
  width: 100%;
  border-radius: 0 8px 8px 0;
  overflow: hidden;
  padding-top: 20px;
  padding-right: 20px;
  height: 100%;
  box-sizing: border-box;
  div {
    height: 100%;
  }
}
</style>
