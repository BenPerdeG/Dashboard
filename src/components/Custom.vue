<template>
  <div class="graph-card">
    <div class="header">
      <div class="info">
        <slot name="icon">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon" viewBox="0 0 24 24" fill="currentColor">
            <path d="M3 3h2v18H3V3zm4 7h2v11H7V10zm4-4h2v15h-2V6zm4 8h2v7h-2v-7zm4-5h2v12h-2V9z"/>
          </svg>
        </slot>
        <div>
          <div class="title">{{ title }}</div>
          <div class="value">{{ value }}</div>
        </div>
      </div>
      <div class="last-value-container">
        <div class="last-value">
          {{ lastBarValue }}
        </div>
      </div>
    </div>

    <VueApexCharts
      class="chart"
      type="bar"
      :height="chartHeight"
      :options="chartOptions"
      :series="chartSeries"
    />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts';
import { computed } from 'vue';

const props = defineProps({
  title: { type: String, default: 'Metric' },
  value: { type: String, default: '0' },
  chartSeries: {
    type: Array as () => Array<{ name?: string, data: number[] }>,
    default: () => ([{ name: 'Data', data: [5, 3, 4, 7, 2] }])
  },
  chartHeight: { type: [Number, String], default: 120 },
  barColor: { type: String, default: '#3b82f6' },
});

const lastBarValue = computed(() => {
  const series = props.chartSeries[0]?.data || [];
  return series.length > 0 ? series[series.length - 1] : 0;
});

const chartOptions = computed(() => ({
  chart: {
    type: 'bar',
    sparkline: { enabled: true },
    toolbar: { show: false }
  },
  plotOptions: {
    bar: {
      borderRadius: 6,
      columnWidth: '70%'
    }
  },
  dataLabels: { enabled: false },
  xaxis: {
    labels: { show: false },
    axisTicks: { show: false },
    axisBorder: { show: false }
  },
  yaxis: {
    labels: { show: false }
  },
  fill: {
    colors: [props.barColor]
  },
  grid: {
    show: false
  },
  tooltip: {
    enabled: false
  }
}));
</script>

<style scoped>
.graph-card {
  display: flex;
  flex-direction: column;
  border-radius: 12px;
  padding: 1rem;
  color: white;
}

.header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.75rem;
}

.info {
  display: flex;
  align-items: center;
  gap: 12px;
}

.icon {
  width: 28px;
  height: 28px;
  color: white;
}

.title {
  font-size: 0.9rem;
  text-transform: uppercase;
  opacity: 0.8;
}

.value {
  font-size: 1.4rem;
  font-weight: bold;
}

.last-value-container {
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  padding: 0.5rem 0.75rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.last-value {
  font-size: 1.2rem;
  font-weight: bold;
}

.chart {
  width: 100%;
}

/* Theme Colors */
.bg-default {
  background: linear-gradient(135deg, #1e3a8a, #3b82f6);
}

.bg-success {
  background: linear-gradient(135deg, #14532d, #22c55e);
}

.bg-warning {
  background: linear-gradient(135deg, #92400e, #f59e0b);
}

.bg-danger {
  background: linear-gradient(135deg, #7f1d1d, #ef4444);
}
</style>