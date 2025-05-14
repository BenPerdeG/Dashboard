<template>
  <div :class="['box-sparkline', bgColor, textColor]">
    <div class="details">
      <div class="title-container">
        <ion-icon :name="iconName"></ion-icon>
        <span class="title-text">{{title}}</span>  
      </div>
      <span class="value-text">{{value}}</span>
    </div>
    <vapexChart 
      class="sparkline-chart"
      :height="chartHeight" 
      :options="chartOptions"
      :series="chartSeries">
    </vapexChart>
  </div>
</template>

<script setup lang="ts">
import { IonIcon } from '@ionic/vue';
import { addIcons } from 'ionicons';
import { navigateOutline, logoIonic, eyeOutline, peopleOutline, cashOutline, trendingUpOutline, trendingDownOutline, statsChartOutline, logInOutline } from 'ionicons/icons';
import vapexChart from 'vue3-apexcharts';
import { ref, watchEffect, onUnmounted } from 'vue';

// 📌 Registrar iconos
addIcons({
  'logo-ionic': logoIonic,
  'navigate-outline': navigateOutline,
  'eye-outline': eyeOutline,
  'people-outline': peopleOutline,
  'cash-outline': cashOutline,
  'trending-up-outline': trendingUpOutline,
  'trending-down-outline': trendingDownOutline,
  'stats-chart-outline': statsChartOutline,
  'log-in-outline': logInOutline
});

// 📌 Definir Props para datos dinámicos
defineProps({
  title: { type: String, default: 'Métrica' },
  value: { type: String, default: '#Value' },
  chartOptions: { type: Object, required: true },
  chartSeries: { type: Array, required: true },
  bgColor: { type: String, default: 'gradient-blue' },
  textColor: { type: String, default: 'white' },
  iconName: { type: String, default: 'stats-chart-outline' },
});

/******* Control altura gráfico según ancho ********************/

const chartHeight = ref("35%");

// Función que ajusta la altura dinámicamente
const updateChartHeight = () => {
  const width = window.innerWidth;

  if (width < 576) chartHeight.value = "20%"; // Breakpoint xs
  else if (width < 768) chartHeight.value = "25%"; // Breakpoint sm
  else chartHeight.value = "35%"; // Breakpoint md y superiores
};

// Ejecutar al cargar y escuchar cambios en el tamaño de la ventana
watchEffect(() => {
  updateChartHeight();
  window.addEventListener("resize", updateChartHeight);
});

// Limpiar el listener cuando el componente se desmonta
onUnmounted(() => {
  window.removeEventListener("resize", updateChartHeight);
});

/************************************************************ */
</script>

<style scoped>
/* Mobile first */

.box-sparkline {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 75%;
  width: 100%;
  padding: 8px;
  border-radius: 6px;
  container: box / inline-size;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.2s ease;
  position: relative;
  overflow: hidden;
}

.box-sparkline::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle at top right, rgba(255, 255, 255, 0.1) 0%, transparent 60%);
  pointer-events: none;
}

.details {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  margin-bottom: 4px;
}

.title-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
}

.title-container ion-icon {
  font-size: 1rem;
  --ionicon-stroke-width: 32px;
}

.title-text {
  font-size: 0.65rem;
  font-weight: 500;
  letter-spacing: 0.2px;
  text-transform: uppercase;
  opacity: 0.9;
}

.value-text {
  font-size: 1.8rem;
  font-weight: 700;
  letter-spacing: -0.5px;
  line-height: 1;
}

.sparkline-chart {
  min-width: 50px;
  width: 100%;
  margin-top: 4px;
}

/* Siendo más ancho, pasamos dato a la derecha de título */
@container box (width >= 200px) {
  .details {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    gap: 6px;
  }

  .title-container {
    flex-direction: row;
    gap: 6px;
    align-items: center;
  }

  .value-text {
    font-size: 4cqmax;
  }

  .title-container ion-icon {
    font-size: 2.5cqmax;
  }

  .title-text {
    font-size: 1.5cqmax;
  }
}

/* 🖥️ En pantallas grandes (>=lg=992) */
@media (min-width: 992px) {
  /* Si el componente no es muy ancho: detalles izquierda, datos derecha*/
  @container box (width <= 280px) {
    .details {
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      gap: 6px;
    }
    /* Para anchos de contenedores muy pequeños */
    .value-text {
      font-size: max(1.2rem, 12cqw);
    }
    .title-container ion-icon {
      font-size: max(0.9rem, 3.5cqw);
    }
    .title-text {
      font-size: max(0.6rem, 3.5cqw);
    }
  }
}

/* 🎨 Colores de fondo mejorados */
.gradient-blue {
  background-image: linear-gradient(135deg, #071c49 10%, #0396FF 100%);
  border-left: 2px solid #0396FF;
}

.gradient-green {
  background-image: linear-gradient(135deg, #054d43 10%, #6be084 100%);
  border-left: 2px solid #6be084;
}

.gradient-orange {
  background-image: linear-gradient(135deg, #5f1b2d 10%, #e78f30 100%);
  border-left: 2px solid #e78f30;
}

.gradient-pink {
  background-image: linear-gradient(135deg, #383ead 10%, #EE9AE5 100%);
  border-left: 2px solid #EE9AE5;
}

.black { color: black; }
.white { color: white; }
</style>
