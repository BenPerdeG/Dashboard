<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar class="header-toolbar">
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title class="dashboard-title">
          <IonIcon :icon="trendingUpOutline" class="title-icon" />
          Dashboard de Negocio
        </ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding dashboard-content scrollable-content">
      <ion-grid class="dashboard-grid">

        <!-- Row 1: ECharts pie + Apex bar -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="4">
            <div class="dashboard-card pie-chart-card">
              <div class="card-header">
                <IonIcon :icon="peopleOutline" class="card-icon" />
                <h3 class="card-title">Distribución de Usuarios</h3>
              </div>
              <VChart :option="echartPieOptions" class="echart-full small-height" autoresize />
            </div>
          </ion-col>
          <ion-col size="12" size-md="8">
            <div class="dashboard-card bar-chart-card">
              <div class="card-header">
                <IonIcon :icon="barChartOutline" class="card-icon" />
                <h3 class="card-title">Horas mensuales jugadas de media</h3>
              </div>
              <VueApexCharts type="bar" :options="apexOptions" :series="apexSeries" width="100%" height="160" />
            </div>
          </ion-col>
        </ion-row>

        <!-- Row 2: Custom Component + Gauge -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="6">
            <div class="custom dashboard-card gauge-card">
              <Component 
                title="Registros de nuevos usuarios " 
                value="(Últimos 10 dias)" 
                textColor="white"
                iconName="navigate-outline" 
                :chartSeries="[{ data: [8, 6, 5, 9, 5, 4, 2, 6, 5, 9] }]"
                chartHeight="160" 
              />
            </div>
          </ion-col>
          <ion-col size="12" size-md="6">
            <div class="dashboard-card gauge-card">
              <div class="card-header">
                <IonIcon :icon="speedometerOutline" class="card-icon" />
                <h3 class="card-title">Usuarios creando partidas</h3>
              </div>
              <div class="gauge-container small-height">
                <Chart :type="'doughnut'" :data="gaugeData" :options="gaugeOptions" />
                <div class="gauge-percentage">0%</div>
              </div>
            </div>
          </ion-col>
        </ion-row>

        <!-- Row 3: Real-time -->
        <ion-row class="dashboard-row">
          <ion-col size="12">
            <div class="dashboard-card realtime-card">
              <div class="card-header">
                <IonIcon :icon="pulseOutline" class="card-icon" />
                <h3 class="card-title">Usuarios activos</h3>
                <div class="realtime-badge">
                  <span class="pulse-dot"></span>
                  En vivo
                </div>
              </div>
              <VueApexCharts type="line" :options="realTimeOptions" :series="[{ name: 'Live', data: realTimeData }]"
                width="100%" height="160" />
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
import type { ChartOptions } from 'chart.js';


// Data and Options
const gaugeData = {
  labels: ['Usuarios que han creado una partida', 'Usuarios que NO han creado una partida'],
  datasets: [{
    data: [1, 99],
    backgroundColor: ['#f59e0b', '#1f2937'],
    borderWidth: 0,
    borderRadius: 5
  }]
};

const gaugeOptions: ChartOptions<'doughnut'> = {
  rotation: -90,
  circumference: 180,
  cutout: '80%',
  plugins: {
    legend: { display: false },
    tooltip: {
      callbacks: {
        label: (context) => `${context.label}: ${context.raw}%`
      }
    }
  },
  animation: {
    animateRotate: true,
    animateScale: true
  }
};
const apexOptions = {
  chart: {
    type: 'bar',
    dropShadow: {
      enabled: true,
      top: 2,
      left: 2,
      blur: 4,
      opacity: 0.2
    }
  },
  plotOptions: {
    bar: {
      borderRadius: 6,
      columnWidth: '60%',
      dataLabels: {
        position: 'top'
      }
    }
  },
  dataLabels: {
    enabled: true,
    offsetY: -20,
    style: {
      fontSize: '12px',
      colors: ["#fff"]
    }
  },
  colors: ['#3b82f6', '#6366f1', '#8b5cf6', '#a855f7', '#d946ef'],
  xaxis: {
    categories: ['Ene', 'Feb', 'Mar', 'Abr', 'May'],
    labels: {
      style: {
        colors: '#cbd5e1',
        fontSize: '12px'
      }
    },
    axisBorder: { show: false },
    axisTicks: { show: false }
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
    y: { formatter: (val: string) => val + " unidades" }
  }
};
const apexSeries = [{ name: 'Horas', data: [28, 20, 25, 20, 12] }];

const echartPieOptions = {
  title: {
    text: 'Tipos de Usuario',
    textStyle: {
      color: '#f8fafc',
      fontSize: 14,
      fontWeight: 'normal'
    },
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
    name: 'Usuarios',
    type: 'pie',
    radius: ['40%', '70%'],
    center: ['65%', '50%'],
    avoidLabelOverlap: false,
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
      { value: 2, name: 'Admins', itemStyle: { color: '#3b82f6' } },
      { value: 12, name: 'Jugadores', itemStyle: { color: '#f59e0b' } }
    ]
  }]
};

const realTimeData = ref([5, 7, 6, 6, 8, 7, 5]);


function generateRandomData() {
  const min = 3;
  const max = 10;
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

setInterval(() => {

  if (realTimeData.value.length > 12) {
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
    animations: {
      enabled: true,
      easing: 'linear',
      dynamicAnimation: { speed: 1000 }
    },
    toolbar: { show: false },
    dropShadow: {
      enabled: true,
      top: 3,
      left: 3,
      blur: 4,
      opacity: 0.2
    }
  },
  colors: ['#f59e0b'],
  stroke: { curve: 'smooth', width: 3 },
  markers: { size: 0 },
  fill: {
    type: 'gradient',
    gradient: {
      shade: 'dark',
      type: 'vertical',
      shadeIntensity: 0.5,
      gradientToColors: ['#f97316'],
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
    max: (max: number) => max + 5
  },
  grid: {
    borderColor: '#334155',
    strokeDashArray: 5,
    padding: { top: 0, right: 0, bottom: 0, left: 0 }
  },
  tooltip: {
    theme: 'dark',
    x: { show: false },
    y: { formatter: (val: string) => val + " usuarios" }
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
  /* Add these to prevent row height issues */
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
  /* Add these to contain charts properly */
  overflow: hidden;
  position: relative;
}

/* Chart container fixes */
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

/* Specific fixes for different chart types */
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

/* Card header styles */
.card-header {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
  /* Prevent header from affecting height */
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

/* Real-time badge styles */
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
/* Ensure columns have consistent height */
ion-col {
  display: flex;
  flex-direction: column;
}

</style>