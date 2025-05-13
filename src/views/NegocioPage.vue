<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>


    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Negocio</ion-title>
        </ion-toolbar>
      </ion-header>

    
      <ion-grid class="dashboard-grid">
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkData1" />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkData1" />
            </div>
          </ion-col>

          <ion-col size="6" size-lg="3">
            <div class="box">Columna 2</div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">Columna 3</div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">Columna 4</div>
          </ion-col>
        </ion-row>


        <!-- 🔵 Fila 2: 2 Columnas -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="9">
            <div class="box">Columna Grande</div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">Columna Pequeña</div>
          </ion-col>
        </ion-row>


        <!-- 🟠 Fila 3: 2 Columnas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="6">
            <div class="box">Columna 1</div>
          </ion-col>
          <ion-col size="12" size-lg="6">
            <div class="box">Columna 2</div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>


<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonGrid, IonRow, IonCol } from '@ionic/vue';
import SparkLine from '../components/SparkLine.vue';

const sparkData1 = {
  /* Propiedades del componente */
  title: "CLICKS",
  value: "1234",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "navigate-outline",
  /* Propiedades del componentes interno de ApexChart */
  chartOptions: {
    chart: {
      id: 'clicks',
      type: 'area',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } }
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
};

</script>


<style scoped>
ion-row {
  overflow: hidden;
}


ion-col {
  max-height: 100%;
  --ion-grid-column-padding: 10px;
}


/* El contenido real de cada columna */
.box {
  background: #1E1E1E;
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: start;
}


/* Aplicar altura total y por filas, solo en pantallas ≥ md */
@media (min-width: 992px) {
  ion-grid {
    height: 100%;
  }

  .ion-row-1 {
    height: 20%;
    max-height: 20%;
  }

  .ion-row-2 {
    height: 40%;
    max-height: 40%;
  }

  .ion-row-3 {
    height: 40%;
    max-height: 40%;
  }
}
</style>