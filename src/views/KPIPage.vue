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
        <ion-accordion v-for="item in businessGoals" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
      <br>
      <h1 class="ion-padding">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in technicalGoals" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
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
  smart: SmartElement[];
}


// Array de objetivos SMART
const technicalGoals = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Reducir inactividad del sistema",
    description: "Reducir el tiempo total de inactividad mensual en un 30% durante los próximos 3 meses, mediante refactorización del backend y alertas tempranas.",
    smart: [
      { letter: "S", content: "Reducir el tiempo total de inactividad mensual en un 30%" },
      { letter: "M", content: "De ~500 horas en mayo a menos de 350 horas para agosto" },
      { letter: "A", content: "Mediante refactorización del backend y alertas tempranas" },
      { letter: "R", content: "Mejorará la experiencia del usuario y estabilidad del sistema" },
      { letter: "T", content: "Durante los próximos 3 meses" }
    ]
  },
  {
    id: 2,
    title: "Disminuir cuentas eliminadas",
    description: "Reducir las cuentas eliminadas en un 40% en los próximos 30 días, identificando las causas principales y corrigiendo errores críticos en UX.",
    smart: [
      { letter: "S", content: "Reducir las cuentas eliminadas en un 40%" },
      { letter: "M", content: "De 21 a menos de 13 eliminaciones diarias promedio" },
      { letter: "A", content: "Identificando las causas y corrigiendo errores UX" },
      { letter: "R", content: "Retendrá usuarios activos y reducirá pérdidas de datos" },
      { letter: "T", content: "En los próximos 30 días" }
    ]
  },
  {
    id: 3,
    title: "Optimizar generación y edición de tokens",
    description: "Mantener la fluctuación de tokens generados/editados por debajo de ±15% respecto a la media horaria, durante las siguientes 2 semanas.",
    smart: [
      { letter: "S", content: "Mantener la variabilidad de tokens en ±15% respecto a la media horaria" },
      { letter: "M", content: "Analizando los datos del gráfico en vivo de tokens" },
      { letter: "A", content: "Implementando normalización de operaciones y control de carga" },
      { letter: "R", content: "Garantiza consistencia en el sistema y reduce errores inesperados" },
      { letter: "T", content: "Durante las próximas 2 semanas" }
    ]
  },
  {
    id: 4,
    title: "Incrementar el uso de partidas privadas",
    description: "Aumentar el porcentaje de partidas privadas de 65% a 80% en 45 días, destacando los beneficios de privacidad en el flujo de creación.",
    smart: [
      { letter: "S", content: "Aumentar el porcentaje de partidas privadas de 65% a 80%" },
      { letter: "M", content: "Incremento del 15% en la proporción actual de partidas" },
      { letter: "A", content: "Modificando interfaz y resaltando beneficios de privacidad" },
      { letter: "R", content: "Protege a usuarios y mejora la percepción de seguridad" },
      { letter: "T", content: "En 45 días" }
    ]
  },
  {
    id: 5,
    title: "Reducir reportes del problema 'No puedo crear partida'",
    description: "Reducir en un 50% los reportes de 'No puedo crear partida' en los próximos 20 días, con mejoras específicas en el flujo de creación y validaciones.",
    smart: [
      { letter: "S", content: "Reducir en un 50% los reportes de 'No puedo crear partida'" },
      { letter: "M", content: "De 700 reportes actuales a menos de 350" },
      { letter: "A", content: "Corrigiendo validaciones y simplificando el flujo de creación" },
      { letter: "R", content: "Soluciona el tipo de reporte más frecuente, mejorando retención" },
      { letter: "T", content: "En los próximos 20 días" }
    ]
  }
]);

const businessGoals = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Aumentar la proporción de usuarios tipo 'Jugador'",
    description: "Aumentar en un 10% la proporción de usuarios tipo 'Jugador' frente a otros roles antes del final del próximo trimestre.",
    smart: [
      { letter: "S", content: "Aumentar en un 10% la proporción de usuarios tipo 'Jugador'" },
      { letter: "M", content: "De 12 a 13 jugadores o más según la gráfica de distribución" },
      { letter: "A", content: "Mediante campañas de adquisición dirigidas a jugadores potenciales" },
      { letter: "R", content: "Permitirá optimizar la orientación del contenido y mejorar la retención" },
      { letter: "T", content: "Antes del final del próximo trimestre" }
    ]
  },
  {
    id: 2,
    title: "Incrementar horas mensuales jugadas por usuario",
    description: "Aumentar en un 20% el promedio mensual de horas jugadas por usuario en los próximos 2 meses.",
    smart: [
      { letter: "S", content: "Incrementar el promedio mensual de horas jugadas por usuario un 20%" },
      { letter: "M", content: "De 21 horas actuales a 25 horas por usuario" },
      { letter: "A", content: "Mediante inclusion de nuevas herramientas u opciones" },
      { letter: "R", content: "Fomentará una mayor retención y compromiso del usuario" },
      { letter: "T", content: "Durante los próximos 2 meses" }
    ]
  },
  {
    id: 3,
    title: "Aumentar registros diarios de nuevos usuarios",
    description: "Elevar en un 30% los registros diarios promedio en un plazo de 6 semanas.",
    smart: [
      { letter: "S", content: "Elevar en un 30% los registros diarios promedio" },
      { letter: "M", content: "Pasar de 5.9 a aproximadamente 7.7 nuevos usuarios diarios" },
      { letter: "A", content: "Mediante mejoras en la landing page y campañas en redes sociales" },
      { letter: "R", content: "Incrementará la base activa de usuarios del sistema" },
      { letter: "T", content: "En un plazo de 6 semanas" }
    ]
  },
  {
    id: 4,
    title: "Aumentar el porcentaje de usuarios que crean partidas",
    description: "Incrementar al 15% el porcentaje de usuarios que crean partidas antes de que finalice el mes siguiente.",
    smart: [
      { letter: "S", content: "Incrementar al 15% el porcentaje de usuarios que crean partidas" },
      { letter: "M", content: "Pasar del 0% actual a al menos 15%" },
      { letter: "A", content: "Añadiendo el botón de crear partida" },
      { letter: "R", content: "Mejorará la generación de contenido interno y la retención" },
      { letter: "T", content: "Antes del final del próximo mes" }
    ]
  },
  {
    id: 5,
    title: "Mantener un mínimo constante de usuarios activos en tiempo real",
    description: "Mantener al menos 7 usuarios activos simultáneamente en promedio, de forma sostenida durante el horario pico (18h-22h), en los próximos 30 días.",
    smart: [
      { letter: "S", content: "Mantener al menos 7 usuarios activos simultáneamente durante el horario pico" },
      { letter: "M", content: "Promedio sostenido ≥ 7 usuarios entre las 18h y 22h" },
      { letter: "A", content: "Mediante notificaciones, eventos y recordatorios en horas clave" },
      { letter: "R", content: "Fomentará el dinamismo del juego y percepción de comunidad" },
      { letter: "T", content: "Durante los próximos 30 días" }
    ]
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
