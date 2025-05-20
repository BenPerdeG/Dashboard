<template>
    <ion-page>
      <ion-header :translucent="true">
        <ion-toolbar>
          <ion-buttons slot="start">
            <ion-menu-button color="primary"></ion-menu-button>
          </ion-buttons>
          <ion-title>🎯 KPIs</ion-title>
        </ion-toolbar>
      </ion-header>
  
  
      <ion-content :fullscreen="true" class="ion-padding">
        <ion-header collapse="condense">
          <ion-toolbar>
            <ion-title size="large">🎯 KPIs</ion-title>
          </ion-toolbar>
        </ion-header>
  
  
        <h1 class="ion-padding">🚀 KPIs de Negocio</h1>
        <ion-accordion-group expand="inset" :multiple="true">
          <ion-accordion v-for="item in smartGoals" :key="item.id" :value="item.id.toString()">
            <ion-item slot="header">
              <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
            </ion-item>
            <div class="ion-padding" slot="content">
              <p>{{ item.description }}</p>
            </div>
          </ion-accordion>
        </ion-accordion-group>
        <br>
        <h1 class="ion-padding">📈 KPIs Técnicos</h1>
        <ion-accordion-group expand="inset" :multiple="true">
          <ion-accordion v-for="item in smartGoals" :key="item.id" :value="item.id.toString()">
            <ion-item slot="header">
              <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
            </ion-item>
            <div class="ion-padding" slot="content">
              <p>{{ item.description }}</p>
            </div>
          </ion-accordion>
        </ion-accordion-group>
      </ion-content>
    </ion-page>
  </template>
  
  
  <script setup lang="ts">
  import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonAccordionGroup, IonAccordion, IonItem, IonLabel, IonList } from '@ionic/vue';
  import { ref } from 'vue';
  
  
  // Definición de la estructura de datos para un objetivo SMART
  interface SmartElement {
    letter: string;
    content: string;
  }
  
  
  interface SmartGoal {
    id: number;
    title: string;
    description: string;
  }
  
  
  // Array de objetivos SMART
  const smartGoals = ref<SmartGoal[]>([
    {
      id: 1,
      title: "Aumentar visitas",
      description: "Aumentar las visitas de nuestro sitio web en un 50% (de 1000 a 1500) en los siguientes 30 días, al duplicar la distribución de contenido y con el fin de prepararnos para el lanzamiento de nuestro nuevo producto",
    }, {
      id: 2,
      title: "Aumentar visitas",
      description: "Aumentar las visitas de nuestro sitio web en un 50% (de 1000 a 1500) en los siguientes 30 días, al duplicar la distribución de contenido y con el fin de prepararnos para el lanzamiento de nuestro nuevo producto",
    }, {
      id: 3,
      title: "Aumentar visitas",
      description: "Aumentar las visitas de nuestro sitio web en un 50% (de 1000 a 1500) en los siguientes 30 días, al duplicar la distribución de contenido y con el fin de prepararnos para el lanzamiento de nuestro nuevo producto",
    }, {
      id: 4,
      title: "Aumentar visitas",
      description: "Aumentar las visitas de nuestro sitio web en un 50% (de 1000 a 1500) en los siguientes 30 días, al duplicar la distribución de contenido y con el fin de prepararnos para el lanzamiento de nuestro nuevo producto",
    }, {
      id: 5,
      title: "Aumentar visitas",
      description: "Aumentar las visitas de nuestro sitio web en un 50% (de 1000 a 1500) en los siguientes 30 días, al duplicar la distribución de contenido y con el fin de prepararnos para el lanzamiento de nuestro nuevo producto",
    }
  ]);
  
  
  </script>
  
  
  <style scoped>
  
  
  ion-accordion.accordion-collapsing ion-item[slot='header'],
    ion-accordion.accordion-collapsed ion-item[slot='header'] {
      --background: var(--ion-color-light);
      --color: var(--ion-color-light-contrast);
    }
  
  
    ion-accordion.accordion-expanding ion-item[slot='header'],
    ion-accordion.accordion-expanded ion-item[slot='header'] {
      --background: rgba(var(--ion-color-primary-rgb), 0.14);
      color: var(--ion-color-primary);
    }
  
  
  </style>
  