<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">

      <ion-grid class="dashboard-grid">

        <!-- 🟢 Fila 1: métricas -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box">
              <SparkLine
                title="Ventas Hoy"
                :value="'€1,250'"
                :chartOptions="ventasChartOptions"
                :chartSeries="ventasChartSeries"
                bgColor="gradient-green"
                textColor="white"
                iconName="cash-outline"
              />
            </div>
          </ion-col>

          <ion-col size="6" size-lg="3">
            <div class="box">
              <SparkLine
                title="Pedidos"
                :value="'87'"
                :chartOptions="pedidosChartOptions"
                :chartSeries="pedidosChartSeries"
                bgColor="gradient-blue"
                textColor="white"
                iconName="navigate-outline"
              />
            </div>
          </ion-col>

          <ion-col size="6" size-lg="3">
            <div class="box">
              <SparkLine
                title="Ticket Promedio"
                :value="'€14.37'"
                :chartOptions="ticketChartOptions"
                :chartSeries="ticketChartSeries"
                bgColor="gradient-orange"
                textColor="white"
                iconName="eye-outline"
              />
            </div>
          </ion-col>

          <ion-col size="6" size-lg="3">
            <div class="box">
              <SparkLine
                title="Clientes"
                :value="'52'"
                :chartOptions="clientesChartOptions"
                :chartSeries="clientesChartSeries"
                bgColor="gradient-pink"
                textColor="white"
                iconName="people-outline"
              />
            </div>
          </ion-col>
        </ion-row>

        <!-- 🔵 Fila 2: gráfico grande -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="8">
            <div class="box">
              <ApexMixedChart :series="dataMixedChartSeries" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4">
            <div class="box">
              <EchartsGauge :value="gaugeRendimiento" title="RENDIMIENTO" />
            </div>
          </ion-col>
        </ion-row>

       <!-- En el <template>, dentro de ion-row-3 -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="6">
            <div class="box">
              <EchartsMap
                :data="dataDownloadsEU"
                title="Descargas UE"
                subtitle="KPI: 8 Países > 1K"
              />
            </div>
          </ion-col>
        </ion-row>
      
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent, IonHeader, IonPage, IonTitle,
  IonToolbar, IonGrid, IonRow, IonCol
} from '@ionic/vue';
import EchartsGauge from '@/components/EcharstGauge.vue';
import SparkLine from '@/components/SparkLine.vue';
import ApexMixedChart from '@/components/ApexMixedChart.vue';
import EchartsMap from '@/components/EcharstMap.vue';
import { ref } from 'vue';

/* =========================
   📊 Sparkline data
========================= */

const ventasChartOptions = {
  chart: { sparkline: { enabled: true } },
  stroke: { curve: 'smooth', width: 2 },
  colors: ['#6be084'],
  tooltip: { enabled: false }
};
const ventasChartSeries = [{ data: [120, 150, 180, 200, 170, 220, 250] }];

const pedidosChartOptions = {
  chart: { sparkline: { enabled: true } },
  stroke: { curve: 'smooth', width: 2 },
  colors: ['#0396FF'],
  tooltip: { enabled: false }
};
const pedidosChartSeries = [{ data: [10, 12, 15, 13, 16, 18, 20] }];

const ticketChartOptions = {
  chart: { sparkline: { enabled: true } },
  stroke: { curve: 'smooth', width: 2 },
  colors: ['#e78f30'],
  tooltip: { enabled: false }
};
const ticketChartSeries = [{ data: [12.5, 13.2, 14.1, 13.8, 15.0, 14.7, 14.37] }];

const clientesChartOptions = {
  chart: { sparkline: { enabled: true } },
  stroke: { curve: 'smooth', width: 2 },
  colors: ['#EE9AE5'],
  tooltip: { enabled: false }
};
const clientesChartSeries = [{ data: [30, 35, 40, 45, 50, 48, 52] }];

/* =========================
   📊 Mixed Chart data
========================= */

const dataMixedChartSeries = ref([
  {
    name: 'Ventas',
    type: 'column',
    data: [23, 11, 22, 27, 13, 22, 37, 21, 44, 22, 30]
  },
  {
    name: 'Pedidos',
    type: 'area',
    data: [44, 55, 41, 67, 22, 43, 21, 41, 56, 27, 43]
  },
  {
    name: 'Clientes',
    type: 'line',
    data: [30, 25, 36, 30, 45, 35, 64, 52, 59, 36, 39]
  }
]);
const gaugeDescargas = ref(70);
const gaugeRendimiento = ref(45);
const gaugeSatisfaccion = ref(90);

const dataDownloadsEU = ref([
  { name: "Germany",     value: 15000 },
  { name: "France",      value: 12000 },
  { name: "Spain",       value: 16000 },
  { name: "Italy",       value:  9000 },
  { name: "Netherlands", value:  8000 },
  { name: "Poland",      value:  7500 },
  { name: "Portugal",    value:  3000 },
]);
</script>

<style scoped>
ion-row{
  overflow: hidden;
}

ion-col {
  max-height: 100%;
  --ion-grid-column-padding:10px;
}

/* Caja */
.box {
  background: #1E1E1E;
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius:5px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* Layout responsive */
@media (min-width: 992px) {  
  ion-grid{height: 100%;}
  .ion-row-1{height: 20%; max-height: 20%;}
  .ion-row-2{height: 40%; max-height: 40%;}
  .ion-row-3{height: 40%; max-height: 40%;}
}
</style>