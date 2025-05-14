<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar class="header-toolbar">
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title class="dashboard-title">
          <ion-icon name="trending-up-outline" class="title-icon"></ion-icon>
          Dashboard de Negocio
        </ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding dashboard-content">
      <ion-grid class="dashboard-grid">

        <!-- Fila 1: ECharts pie + Apex bar (Orden cambiado) -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="4">
            <div class="dashboard-card pie-chart-card">
              <div class="card-header">
                <ion-icon name="people-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Distribución de Usuarios</h3>
              </div>
              <VChart :option="echartPieOptions" class="echart-full" autoresize />
            </div>
          </ion-col>
          <ion-col size="12" size-md="8">
            <div class="dashboard-card bar-chart-card">
              <div class="card-header">
                <ion-icon name="bar-chart-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Ventas Mensuales</h3>
              </div>
              <VueApexCharts type="bar" :options="apexOptions" :series="apexSeries" width="100%" height="100%" />
            </div>
          </ion-col>
        </ion-row>

        <!-- Fila 2: Sparkline + Gauge (Orden original) -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="6">
            <div class="dashboard-card sparkline-card">
              <div class="card-header">
                <ion-icon name="navigate-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Clicks Totales</h3>
              </div>
              <spark-line v-bind="sparkData1" />
            </div>
          </ion-col>
          <ion-col size="12" size-md="6">
            <div class="dashboard-card gauge-card">
              <div class="card-header">
                <ion-icon name="speedometer-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Progreso de Objetivos</h3>
              </div>
              <div class="gauge-container">
                <Chart :type="'doughnut'" :data="gaugeData" :options="gaugeOptions" />
                <div class="gauge-percentage">65%</div>
              </div>
            </div>
          </ion-col>
        </ion-row>

        <!-- Fila 3: Real-time (Orden original) -->
        <ion-row class="dashboard-row">
          <ion-col size="12">
            <div class="dashboard-card realtime-card">
              <div class="card-header">
                <ion-icon name="pulse-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Actividad en Tiempo Real</h3>
                <div class="realtime-badge">
                  <span class="pulse-dot"></span>
                  En vivo
                </div>
              </div>
              <VueApexCharts type="line" :options="realTimeOptions" :series="[{ name: 'Live', data: realTimeData }]"
                width="100%" height="200" />
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
import { trendingUpOutline, navigateOutline, peopleOutline, barChartOutline, pulseOutline, speedometerOutline } from 'ionicons/icons';

import SparkLine from '../components/SparkLine.vue';

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

// SparkLine props
const sparkData1 = {
  title: "CLICKS",
  value: "1,234",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "navigate-outline",
  chartOptions: {
    chart: {
      id: 'clicks',
      type: 'area',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#4ade80'],
    fill: {
      type: 'gradient',
      gradient: {
        shade: 'dark',
        type: 'vertical',
        shadeIntensity: 0.5,
        gradientToColors: ['#22c55e'],
        inverseColors: false,
        opacityFrom: 0.7,
        opacityTo: 0.3,
      }
    },
    tooltip: { 
      theme: 'dark', 
      x: { show: false }, 
      y: { title: { formatter: () => '' } },
      style: {
        fontSize: '12px',
        fontFamily: 'Inter, sans-serif'
      }
    }
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
};

// Gauge-like doughnut chart
const gaugeData = {
  labels: ['Progreso', 'Restante'],
  datasets: [{
    data: [65, 35],
    backgroundColor: ['#f59e0b', '#1f2937'],
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
        label: function(context) {
          return context.label + ': ' + context.raw + '%';
        }
      }
    }
  },
  animation: {
    animateRotate: true,
    animateScale: true
  }
};

// Apex bar
const apexOptions = {
  chart: { 
    type: 'bar',
    toolbar: {
      show: true,
      tools: {
        download: true,
        selection: false,
        zoom: false,
        zoomin: false,
        zoomout: false,
        pan: false,
        reset: false
      }
    },
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
      distributed: false,
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
    axisBorder: {
      show: false
    },
    axisTicks: {
      show: false
    }
  },
  yaxis: {
    labels: {
      style: {
        colors: '#cbd5e1'
      }
    }
  },
  grid: {
    borderColor: '#334155',
    strokeDashArray: 5,
    padding: {
      top: 0,
      right: 0,
      bottom: 0,
      left: 10
    }
  },
  tooltip: {
    theme: 'dark',
    y: {
      formatter: function(val) {
        return val + " unidades"
      }
    }
  }
};
const apexSeries = [{ name: 'Ventas', data: [10, 41, 35, 51, 49] }];

// ECharts pie
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
    textStyle: {
      color: '#f8fafc'
    }
  },
  legend: { 
    orient: 'vertical',
    left: 'left',
    top: 'middle',
    textStyle: {
      color: '#cbd5e1'
    }
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
    label: {
      show: false
    },
    emphasis: {
      label: {
        show: true,
        fontSize: '14',
        fontWeight: 'bold'
      },
      itemStyle: {
        shadowBlur: 10,
        shadowOffsetX: 0,
        shadowColor: 'rgba(0, 0, 0, 0.5)'
      }
    },
    labelLine: {
      show: false
    },
    data: [
      { value: 1048, name: 'Admins', itemStyle: { color: '#3b82f6' } },
      { value: 735, name: 'Jugadores', itemStyle: { color: '#f59e0b' } },
      { value: 580, name: 'Invitados', itemStyle: { color: '#10b981' } }
    ]
  }]
};

// Real-time
const realTimeData = ref([10, 12, 14, 13, 15, 14, 13])
setInterval(() => {
  const last = realTimeData.value[realTimeData.value.length - 1]
  const next = last + (Math.random() * 4 - 2)
  if (realTimeData.value.length > 20) realTimeData.value.shift()
  realTimeData.value.push(Math.round(next))
}, 1000)

const realTimeOptions = {
  chart: {
    animations: { 
      enabled: true,
      easing: 'linear',
      dynamicAnimation: {
        speed: 1000
      }
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
  stroke: { 
    curve: 'smooth',
    width: 3
  },
  markers: {
    size: 0
  },
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
    max: function(max) { return max + 5 }
  },
  grid: {
    borderColor: '#334155',
    strokeDashArray: 5,
    padding: {
      top: 0,
      right: 0,
      bottom: 0,
      left: 0
    }
  },
  tooltip: {
    theme: 'dark',
    x: { show: false },
    y: {
      formatter: function(val) {
        return val + " usuarios"
      }
    }
  }
}
</script>

<style scoped>
.header-toolbar {
  --background: #0f172a;
  --border-color: #1e293b;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.dashboard-title {
  display: flex;
  align-items: center;
  font-weight: 600;
  color: #f8fafc;
  font-size: 1.25rem;
}

.title-icon {
  margin-right: 8px;
  font-size: 1.5rem;
  color: #f59e0b;
}

.dashboard-content {
  --background: #0f172a;
}

.dashboard-grid {
  padding: 0;
}

.dashboard-row {
  margin-bottom: 16px;
}

.dashboard-card {
  background: #1e293b;
  border-radius: 12px;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  padding: 16px;
  height: 100%;
  transition: transform 0.2s, box-shadow 0.2s;
  border: 1px solid #334155;
  overflow: hidden;
}

.dashboard-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.card-header {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
  padding-bottom: 12px;
  border-bottom: 1px solid #334155;
  position: relative;
}

.card-icon {
  font-size: 1.5rem;
  margin-right: 12px;
  color: #f59e0b;
  background: rgba(245, 158, 11, 0.1);
  padding: 8px;
  border-radius: 8px;
}

.card-title {
  margin: 0;
  font-size: 1rem;
  font-weight: 600;
  color: #f8fafc;
}

.sparkline-card .card-icon {
  color: #3b82f6;
  background: rgba(59, 130, 246, 0.1);
}

.gauge-card .card-icon {
  color: #f59e0b;
  background: rgba(245, 158, 11, 0.1);
}

.bar-chart-card .card-icon {
  color: #6366f1;
  background: rgba(99, 102, 241, 0.1);
}

.pie-chart-card .card-icon {
  color: #10b981;
  background: rgba(16, 185, 129, 0.1);
}

.realtime-card .card-icon {
  color: #ef4444;
  background: rgba(239, 68, 68, 0.1);
}

.gauge-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 250px;
}

.gauge-percentage {
  position: absolute;
  font-size: 2rem;
  font-weight: 700;
  color: #f8fafc;
}

.echart-full {
  width: 100%;
  height: 250px;
}

.realtime-badge {
  position: absolute;
  right: 0;
  top: 0;
  background: rgba(239, 68, 68, 0.15);
  color: #ef4444;
  font-size: 0.75rem;
  padding: 4px 8px;
  border-radius: 4px;
  display: flex;
  align-items: center;
}

.pulse-dot {
  height: 8px;
  width: 8px;
  background-color: #ef4444;
  border-radius: 50%;
  margin-right: 6px;
  display: inline-block;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(0.95);
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
  }
  
  70% {
    transform: scale(1);
    box-shadow: 0 0 0 6px rgba(239, 68, 68, 0);
  }
  
  100% {
    transform: scale(0.95);
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
  }
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .dashboard-card {
    margin-bottom: 16px;
  }
  
  .card-header {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .card-icon {
    margin-bottom: 8px;
  }
  
  .realtime-badge {
    position: relative;
    margin-top: 8px;
    align-self: flex-start;
  }
}
</style>
