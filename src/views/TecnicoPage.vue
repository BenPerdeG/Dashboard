<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar class="header-toolbar">
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title class="dashboard-title">
          <ion-icon name="analytics-outline" class="title-icon"></ion-icon>
          Dashboard Técnico
        </ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding dashboard-content">
      <ion-grid class="dashboard-grid">

        <!-- Fila 1: Sparkline + Gauge -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="6">
            <div class="dashboard-card sparkline-card">
              <div class="card-header">
                <ion-icon name="log-in-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Inicios de Sesión</h3>
              </div>
              <spark-line v-bind="sparkData1" />
            </div>
          </ion-col>
          <ion-col size="12" size-md="6">
            <div class="dashboard-card gauge-card">
              <div class="card-header">
                <ion-icon name="rocket-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Progreso de Despliegue</h3>
              </div>
              <div class="gauge-container">
                <Chart :type="'doughnut'" :data="gaugeData" :options="gaugeOptions" />
                <div class="gauge-percentage">78%</div>
              </div>
            </div>
          </ion-col>
        </ion-row>

        <!-- Fila 2: Apex bar + ECharts pie -->
        <ion-row class="dashboard-row">
          <ion-col size="12" size-md="8">
            <div class="dashboard-card bar-chart-card">
              <div class="card-header">
                <ion-icon name="bar-chart-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Solicitudes HTTP por Módulo</h3>
              </div>
              <VueApexCharts type="bar" :options="apexOptions" :series="apexSeries" width="100%" height="100%" />
            </div>
          </ion-col>
          <ion-col size="12" size-md="4">
            <div class="dashboard-card pie-chart-card">
              <div class="card-header">
                <ion-icon name="people-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Tipos de Usuario Activos</h3>
              </div>
              <VChart :option="echartPieOptions" class="echart-full" autoresize />
            </div>
          </ion-col>
        </ion-row>

        <!-- Fila 3: Real-time -->
        <ion-row class="dashboard-row">
          <ion-col size="12">
            <div class="dashboard-card realtime-card">
              <div class="card-header">
                <ion-icon name="pulse-outline" class="card-icon"></ion-icon>
                <h3 class="card-title">Conexiones Activas en Tiempo Real</h3>
                <div class="realtime-badge">
                  <span class="pulse-dot"></span>
                  En vivo
                </div>
              </div>
              <VueApexCharts type="line" :options="realTimeOptions" :series="[{ name: 'Conexiones', data: realTimeData }]"
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
import { analyticsOutline, logInOutline, rocketOutline, barChartOutline, peopleOutline, pulseOutline } from 'ionicons/icons';

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

// SparkLine: Inicios de sesión diarios
const sparkData1 = {
  title: "Logins",
  value: "1,248",
  bgColor: "gradient-green",
  textColor: "white",
  iconName: "log-in-outline",
  chartOptions: {
    chart: {
      id: 'logins',
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
  chartSeries: [{ data: [120, 150, 100, 180, 200, 170, 220, 210, 190, 248] }],
};

// Gauge: Progreso de despliegue
const gaugeData = {
  labels: ['Desplegado', 'Pendiente'],
  datasets: [{
    data: [78, 22],
    backgroundColor: ['#10b981', '#1f2937'],
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

// Apex bar: Solicitudes HTTP por módulo
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
  colors: ['#06b6d4', '#0ea5e9', '#3b82f6', '#6366f1', '#8b5cf6'],
  xaxis: { 
    categories: ['Auth', 'Perfil', 'Tienda', 'Chat', 'Notificaciones'],
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
        return val + " solicitudes"
      }
    }
  }
};
const apexSeries = [{ name: 'Peticiones', data: [230, 190, 340, 120, 260] }];

// ECharts pie: Tipos de usuario activos
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
      { value: 812, name: 'Clientes', itemStyle: { color: '#f59e0b' } },
      { value: 356, name: 'Soporte', itemStyle: { color: '#10b981' } },
      { value: 190, name: 'Administradores', itemStyle: { color: '#3b82f6' } }
    ]
  }]
};

// Real-time: Conexiones activas
const realTimeData = ref([12, 14, 13, 15, 16, 17, 16])
setInterval(() => {
  const last = realTimeData.value[realTimeData.value.length - 1]
  const next = last + (Math.random() * 4 - 2)
  if (realTimeData.value.length > 20) realTimeData.value.shift()
  realTimeData.value.push(Math.max(0, Math.round(next)))
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
  colors: ['#06b6d4'],
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
      gradientToColors: ['#0ea5e9'],
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
        return val + " conexiones"
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
  color: #3b82f6;
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
  color: #3b82f6;
  background: rgba(59, 130, 246, 0.1);
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
  color: #10b981;
  background: rgba(16, 185, 129, 0.1);
}

.gauge-card .card-icon {
  color: #f59e0b;
  background: rgba(245, 158, 11, 0.1);
}

.bar-chart-card .card-icon {
  color: #06b6d4;
  background: rgba(6, 182, 212, 0.1);
}

.pie-chart-card .card-icon {
  color: #8b5cf6;
  background: rgba(139, 92, 246, 0.1);
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
