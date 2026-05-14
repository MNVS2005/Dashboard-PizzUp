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

// ── KPIs de Negocio (datos reales de NegocioPage) ──────────────────
const businessGoals = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Ventas diarias",
    description: "Mantener las ventas diarias entre €500 y €3.000, con un valor actual de €1.250, actualizándose cada 2 segundos para reflejar el rendimiento en tiempo real.",
    smart: [
      { letter: "S", content: "Ventas diarias monitorizadas en tiempo real" },
      { letter: "M", content: "Rango objetivo: €500 – €3.000 (actual: €1.250)" },
      { letter: "A", content: "Variación controlada de ±€80 por intervalo" },
      { letter: "R", content: "Refleja el rendimiento comercial del día" },
      { letter: "T", content: "Actualización cada 2 segundos" }
    ]
  },
  {
    id: 2,
    title: "Pedidos activos",
    description: "Gestionar entre 20 y 150 pedidos diarios, con un valor actual de 87, para asegurar la capacidad operativa del equipo.",
    smart: [
      { letter: "S", content: "Control del volumen de pedidos activos" },
      { letter: "M", content: "Rango: 20 – 150 pedidos (actual: 87)" },
      { letter: "A", content: "Variación de ±5 pedidos por intervalo" },
      { letter: "R", content: "Garantiza la capacidad de gestión del equipo" },
      { letter: "T", content: "Actualización cada 2 segundos" }
    ]
  },
  {
    id: 3,
    title: "Ticket promedio",
    description: "Mantener el ticket medio entre €8 y €25, con un valor actual de €14,37, como indicador de la calidad del gasto por cliente.",
    smart: [
      { letter: "S", content: "Seguimiento del ticket medio por pedido" },
      { letter: "M", content: "Rango: €8 – €25 (actual: €14,37)" },
      { letter: "A", content: "Variación de ±€0,50 por intervalo" },
      { letter: "R", content: "Indica el valor real generado por cliente" },
      { letter: "T", content: "Actualización cada 2 segundos" }
    ]
  },
  {
    id: 4,
    title: "Clientes activos",
    description: "Mantener entre 10 y 100 clientes activos simultáneamente, con un valor actual de 52, para medir la demanda real del servicio.",
    smart: [
      { letter: "S", content: "Monitorización de clientes activos en tiempo real" },
      { letter: "M", content: "Rango: 10 – 100 clientes (actual: 52)" },
      { letter: "A", content: "Variación de ±3 clientes por intervalo" },
      { letter: "R", content: "Mide la demanda activa del servicio" },
      { letter: "T", content: "Actualización cada 2 segundos" }
    ]
  },
  {
    id: 5,
    title: "Rendimiento general",
    description: "Alcanzar y mantener un rendimiento global del 55% o superior, medido por el gauge de rendimiento del dashboard de negocio.",
    smart: [
      { letter: "S", content: "Rendimiento global del sistema de negocio" },
      { letter: "M", content: "Valor actual: 55% sobre 100%" },
      { letter: "A", content: "Consolidado a partir de ventas, pedidos y clientes" },
      { letter: "R", content: "KPI resumen del estado del negocio" },
      { letter: "T", content: "Evaluación continua en tiempo real" }
    ]
  },
  {
    id: 6,
    title: "Cobertura geográfica UE",
    description: "Superar 1.000 descargas en al menos 8 países de la UE, con España liderando actualmente con 16.000 descargas.",
    smart: [
      { letter: "S", content: "KPI: ≥8 países europeos con más de 1.000 descargas" },
      { letter: "M", content: "España: 16K · Francia: 12K · Alemania: 15K · Italia: 9K · +3 países" },
      { letter: "A", content: "Expansión progresiva por mercado" },
      { letter: "R", content: "Mide la penetración real en el mercado europeo" },
      { letter: "T", content: "Seguimiento continuo en el mapa del dashboard" }
    ]
  }
]);

// ── KPIs Técnicos (datos reales de TécnicoPage) ────────────────────
const technicalGoals = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Usuarios online",
    description: "Mantener entre 10 y 100 usuarios conectados simultáneamente, con KPI objetivo de 70, monitorizados cada segundo mediante gráfica en tiempo real y gauge.",
    smart: [
      { letter: "S", content: "Usuarios conectados al sistema en tiempo real" },
      { letter: "M", content: "KPI objetivo: 70 usuarios (rango: 10 – 100)" },
      { letter: "A", content: "Monitorización con ApexCharts + gauge ECharts" },
      { letter: "R", content: "Detecta picos de carga y caídas de tráfico" },
      { letter: "T", content: "Actualización cada 1 segundo" }
    ]
  },
  {
    id: 2,
    title: "Carga CPU",
    description: "Mantener la carga del servidor por debajo del 100%, con seguimiento en tiempo real mediante gráfica de línea actualizada cada segundo.",
    smart: [
      { letter: "S", content: "Uso de CPU del servidor principal" },
      { letter: "M", content: "Rango: 0% – 100% (alerta si supera 80%)" },
      { letter: "A", content: "Gráfica ChartJS en tiempo real (tipo line)" },
      { letter: "R", content: "Previene sobrecarga y caídas del servicio" },
      { letter: "T", content: "Actualización cada 1 segundo" }
    ]
  },
  {
    id: 3,
    title: "Uso de memoria RAM",
    description: "Mantener el uso de memoria entre 50% y 70%, con alerta visual si se acerca al límite superior.",
    smart: [
      { letter: "S", content: "Consumo de memoria RAM del servidor" },
      { letter: "M", content: "Rango operativo: 50% – 70%" },
      { letter: "A", content: "Gráfica ChartJS en tiempo real (tipo area)" },
      { letter: "R", content: "Evita degradación de rendimiento por falta de memoria" },
      { letter: "T", content: "Actualización cada 1 segundo" }
    ]
  },
  {
    id: 4,
    title: "Tráfico España",
    description: "Monitorizar el tráfico originado en España, con un rango esperado entre 80% y 99% del total de peticiones nacionales.",
    smart: [
      { letter: "S", content: "Porcentaje de tráfico desde España" },
      { letter: "M", content: "Rango: 80% – 99%" },
      { letter: "A", content: "Gauge múltiple ECharts (arco naranja)" },
      { letter: "R", content: "Valida la distribución geográfica del tráfico nacional" },
      { letter: "T", content: "Actualización cada 1 segundo" }
    ]
  },
  {
    id: 5,
    title: "Tráfico mundial",
    description: "Monitorizar el tráfico internacional (resto del mundo), con un rango esperado entre 10% y 30% del total.",
    smart: [
      { letter: "S", content: "Porcentaje de tráfico internacional" },
      { letter: "M", content: "Rango: 10% – 30%" },
      { letter: "A", content: "Gauge múltiple ECharts (arco verde)" },
      { letter: "R", content: "Detecta variaciones en la distribución global del tráfico" },
      { letter: "T", content: "Actualización cada 1 segundo" }
    ]
  },
  {
    id: 6,
    title: "Clicks totales",
    description: "Seguimiento de clicks totales en tiempo real, con un valor de referencia actual de 1.234.",
    smart: [
      { letter: "S", content: "Total de clicks registrados en el sistema" },
      { letter: "M", content: "Valor de referencia actual: 1.234 clicks" },
      { letter: "A", content: "SparkLine tipo area con gradiente azul" },
      { letter: "R", content: "Mide el nivel de interacción activa del usuario" },
      { letter: "T", content: "Seguimiento continuo" }
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