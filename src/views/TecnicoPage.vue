<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar class="header-toolbar">
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title class="dashboard-title">
          <IonIcon :icon="trendingUpOutline" class="title-icon" />
          Dashboard Técnico
        </ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding dashboard-content scrollable-content">
      <ion-grid class="dashboard-grid">
        <!-- Row 1: Real-time + Gauge -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="6">
            <div class="dashboard-card realtime-card">
              <div class="card-header">
                <IonIcon :icon="pulseOutline" class="card-icon" />
                <h3 class="card-title">Tokens siendo generados, eliminados o editados</h3>
                <div class="realtime-badge">
                  <span class="pulse-dot"></span>
                  En vivo
                </div>
              </div>
              <VueApexCharts type="line" :options="realTimeOptions" :series="[{ name: 'Live', data: realTimeData }]"
                width="100%" height="160" />
            </div>
          </ion-col>

          <ion-col size="12" size-md="6">
            <div class="dashboard-card gauge-card">
              <div class="card-header">
                <IonIcon :icon="speedometerOutline" class="card-icon" />
                <h3 class="card-title">Partidas Privadas</h3>
              </div>
              <div class="gauge-container small-height">
                <Chart :type="'doughnut'" :data="gaugeData" :options="gaugeOptions" />
                <div class="gauge-percentage">65%</div>
              </div>
            </div>
          </ion-col>
        </ion-row>

        <!-- Row 2: Bar + CLICKS -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="8">
            <div class="dashboard-card bar-chart-card">
              <div class="card-header">
                <IonIcon :icon="barChartOutline" class="card-icon" />
                <h3 class="card-title">Horas de Inactividad total</h3>
              </div>
              <VueApexCharts type="bar" :options="apexOptions" :series="apexSeries" width="100%" height="160" />
            </div>
          </ion-col>

          <ion-col size="12" size-md="4">
            <div class="custom dashboard-card gauge-card">
              <Component title="Cuentas eliminadas" value="(Últimos 10 dias)" textColor="white" iconName="navigate-outline"
                :chartSeries="[{ data: [5, 6, 11, 9, 5, 14, 12, 6, 19, 21] }]" chartHeight="160" />
            </div>
          </ion-col>
        </ion-row>

        <!-- Row 3: Pie Chart (Users) -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="6" offset-md="3">
            <div class="dashboard-card pie-chart-card">
              <div class="card-header">
                <IonIcon :icon="peopleOutline" class="card-icon" />
                <h3 class="card-title">Problemas reportados</h3>
              </div>
              <VChart :option="echartPieOptions" class="echart-full small-height" autoresize />
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButtons, IonContent, IonHeader, IonMenuButton,
  IonPage, IonTitle, IonToolbar, IonGrid, IonRow, IonCol, IonIcon
} from '@ionic/vue';
import {
  trendingUpOutline, navigateOutline, peopleOutline, barChartOutline,
  pulseOutline, speedometerOutline
} from 'ionicons/icons';

import Component from '../components/Custom.vue';

// Chart.js
import {
  Chart as ChartJS, Title, Tooltip, Legend, ArcElement
} from 'chart.js'
import { Chart } from 'vue-chartjs'
ChartJS.register(Title, Tooltip, Legend, ArcElement);

// ApexCharts
import VueApexCharts from 'vue3-apexcharts'

// ECharts
import VChart from 'vue-echarts'
import * as echarts from 'echarts/core'
import { PieChart } from 'echarts/charts'
import { TitleComponent, TooltipComponent, LegendComponent } from 'echarts/components'
import { CanvasRenderer } from 'echarts/renderers'
echarts.use([PieChart, TitleComponent, TooltipComponent, LegendComponent, CanvasRenderer])

import { ref } from 'vue'

// Colors Updated
const apexOptions = {
  chart: {
    type: 'bar',
    toolbar: { show: false },
    dropShadow: { enabled: true, top: 2, left: 2, blur: 4, opacity: 0.2 }
  },
  plotOptions: {
    bar: {
      borderRadius: 6,
      columnWidth: '60%',
      dataLabels: { position: 'top' }
    }
  },
  dataLabels: {
    enabled: true,
    offsetY: -20,
    style: { fontSize: '12px', colors: ['#fff'] }
  },
  colors: ['#ec4899', '#f43f5e', '#f87171', '#fb923c', '#34d399'],
  xaxis: {
    categories: ['Ene', 'Feb', 'Mar', 'Abr', 'May'],
    labels: { style: { colors: '#cbd5e1', fontSize: '12px' } },
    axisBorder: { show: false }, axisTicks: { show: false }
  },
  yaxis: {
    labels: { style: { colors: '#cbd5e1' } }
  },
  grid: {
    borderColor: '#334155',
    strokeDashArray: 5,
    padding: { top: 0, right: 0, bottom: 0, left: 10 }
  },
  tooltip: {
    theme: 'dark',
    y: { formatter: val => val + " unidades" }
  }
};
const apexSeries = [{ name: 'Horas', data: [100, 410, 350, 510, 490] }];

const echartPieOptions = {
  title: {
    text: 'Problemas Reportados',
    textStyle: { color: '#f8fafc', fontSize: 14, fontWeight: 'normal' },
    left: 'center',
    top: 0
  },
  tooltip: {
    trigger: 'item',
    backgroundColor: '#1e293b',
    borderColor: '#334155',
    textStyle: { color: '#f8fafc' }
  },
  legend: {
    orient: 'vertical',
    left: 'left',
    top: 'middle',
    textStyle: { color: '#cbd5e1' }
  },
  series: [{
    name: 'Problemas',
    type: 'pie',
    radius: ['40%', '70%'],
    center: ['65%', '50%'],
    itemStyle: {
      borderRadius: 10,
      borderColor: '#1e293b',
      borderWidth: 2
    },
    label: { show: false },
    emphasis: {
      label: { show: true, fontSize: '14', fontWeight: 'bold' },
      itemStyle: {
        shadowBlur: 10,
        shadowOffsetX: 0,
        shadowColor: 'rgba(0, 0, 0, 0.5)'
      }
    },
    labelLine: { show: false },
    data: [
      { value: 350, name: 'De rendimiento', itemStyle: { color: '#eab308' } },
      { value: 250, name: 'De navegación', itemStyle: { color: '#358506' } },
      { value: 30, name: 'No replicables', itemStyle: { color: '#10b981' } },
      { value: 700, name: '\"No puedo crear partida\"', itemStyle: { color: '#6366f1' } }
    ]
  }]
};

const gaugeData = {
  labels: ['Privadas', 'Públicas'],
  datasets: [{
    data: [65, 35],
    backgroundColor: ['#14b8a6', '#1f2937'],
    borderWidth: 0,
    borderRadius: 5
  }]
};

const gaugeOptions = {
  rotation: -90,
  circumference: 180,
  cutout: '80%',
  plugins: {
    legend: { display: false },
    tooltip: {
      callbacks: {
        label: ctx => `${ctx.label}: ${ctx.raw}%`
      }
    }
  },
  animation: {
    animateRotate: true,
    animateScale: true
  }
};

const realTimeData = ref([5, 7, 6, 6, 8, 7, 5]);


function generateRandomData() {
  const min = 0;
  const max = 8;
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

setInterval(() => {

  if (realTimeData.value.length > 5) {
    realTimeData.value.shift();
  }
  

  const lastValue = realTimeData.value[realTimeData.value.length - 1];
  const randomChange = Math.random() * 4 - 2; 
  let newValue = lastValue + randomChange;
  

  newValue = Math.max(2, Math.min(15, Math.round(newValue)));
  
  realTimeData.value.push(newValue);
  

  realTimeData.value = [...realTimeData.value];
}, 1000);

const realTimeOptions = {
  chart: {
    animations: { enabled: true, easing: 'linear', dynamicAnimation: { speed: 1000 } },
    toolbar: { show: false },
    dropShadow: { enabled: true, top: 3, left: 3, blur: 4, opacity: 0.2 }
  },
  colors: ['#22d3ee'],
  stroke: { curve: 'smooth', width: 3 },
  markers: { size: 0 },
  fill: {
    type: 'gradient',
    gradient: {
      shade: 'dark',
      type: 'vertical',
      shadeIntensity: 0.5,
      gradientToColors: ['#06b6d4'],
      inverseColors: false,
      opacityFrom: 0.7,
      opacityTo: 0.3,
      stops: [0, 100]
    }
  },
  xaxis: {
    labels: { show: false },
    axisBorder: { show: false },
    axisTicks: { show: false },
    categories: Array(20).fill('')
  },
  yaxis: {
    labels: { show: false },
    min: 0,
    max: max => max + 5
  },
  grid: {
    borderColor: '#334155',
    strokeDashArray: 5,
    padding: { top: 0, right: 0, bottom: 0, left: 0 }
  },
  tooltip: {
    theme: 'dark',
    x: { show: false },
    y: { formatter: val => val + " tokens" }
  }
}
</script>

<style scoped>
ion-page,
ion-content.dashboard-content,
.dashboard-grid {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.scrollable-content {
  overflow-y: auto;
  background-color: #0f172a;
}

.dashboard-row {
  margin-bottom: 16px;
  align-items: stretch;
  min-height: min-content;
}

.dashboard-card {
  background-color: #1e293b;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25);
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  position: relative;
}

.echart-full,
.echart-full.small-height,
.apexcharts-container,
.gauge-container {
  position: relative;
  height: 160px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.gauge-container canvas {
  position: relative;
  z-index: 1;
  max-width: 100%;
  max-height: 100%;
}

.pie-chart-card .echart-full {
  height: 160px !important;
}

.bar-chart-card .apexcharts-container {
  flex: 1;
  min-height: 160px;
}

.gauge-card .gauge-container {
  height: 160px !important;
}

.card-header {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
  flex-shrink: 0;
}

.card-icon {
  font-size: 20px;
  color: #fbbf24;
  margin-right: 8px;
}

.card-title {
  color: #f8fafc;
  font-size: 16px;
  font-weight: 600;
  margin: 0;
}

.realtime-badge {
  margin-left: auto;
  display: flex;
  align-items: center;
  font-size: 12px;
  color: #22c55e;
  font-weight: bold;
  background-color: #14532d;
  padding: 4px 8px;
  border-radius: 9999px;
}

.pulse-dot {
  width: 8px;
  height: 8px;
  background-color: #22c55e;
  border-radius: 50%;
  margin-right: 6px;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(0.9);
    opacity: 0.7;
  }

  50% {
    transform: scale(1.2);
    opacity: 1;
  }

  100% {
    transform: scale(0.9);
    opacity: 0.7;
  }
}

.gauge-percentage {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
  font-size: 20px;
  font-weight: bold;
  color: #fbbf24;
  margin: 0;
  width: max-content;
}

ion-col {
  display: flex;
  flex-direction: column;
}
</style>
