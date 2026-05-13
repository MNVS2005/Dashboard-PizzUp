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
                :value="`€${ventasValor.toFixed(0)}`"
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
                :value="`${pedidosValor.toFixed(0)}`"
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
                :value="`€${ticketValor.toFixed(2)}`"
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
                :value="`${clientesValor.toFixed(0)}`"
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
            <ion-col size="12" size-lg="6">
              <div class="box">
                <UiverseCard title="Mi KPI" value="98%" />
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
import UiverseCard from '@/components/UiverseCard.vue';
import { ref, onUnmounted } from 'vue';

/* =========================
   📊 Sparkline data
========================= */

// En NegocioPage.vue - reemplaza las series estáticas por estas

const ventasChartSeries = ref([{ data: [120, 150, 180, 200, 170, 220, 250] }]);
const pedidosChartSeries = ref([{ data: [10, 12, 15, 13, 16, 18, 20] }]);
const ticketChartSeries  = ref([{ data: [12.5, 13.2, 14.1, 13.8, 15.0, 14.7, 14.37] }]);
const clientesChartSeries = ref([{ data: [30, 35, 40, 45, 50, 48, 52] }]);

// Valores actuales (los que muestran las tarjetas)
const ventasValor   = ref(1250);
const pedidosValor  = ref(87);
const ticketValor   = ref(14.37);
const clientesValor = ref(52);

// Función que genera un nuevo valor con variación aleatoria
const variar = (actual: number, min: number, max: number, delta: number) => {
  const nuevo = actual + (Math.random() * delta * 2 - delta);
  return Math.min(max, Math.max(min, nuevo));
};

// Actualiza una serie: quita el primer dato, añade uno nuevo al final
const pushDato = (series: typeof ventasChartSeries, nuevoValor: number) => {
  const datos = [...series.value[0].data.slice(1), Math.round(nuevoValor * 100) / 100];
  series.value = [{ data: datos }];
};

// Intervalo cada 2 segundos
const intervalo = setInterval(() => {
  ventasValor.value   = variar(ventasValor.value,   500,  3000, 80);
  pedidosValor.value  = variar(pedidosValor.value,  20,   150,  5);
  ticketValor.value   = variar(ticketValor.value,   8,    25,   0.5);
  clientesValor.value = variar(clientesValor.value, 10,   100,  3);

  pushDato(ventasChartSeries,  ventasValor.value);
  pushDato(pedidosChartSeries, pedidosValor.value);
  pushDato(ticketChartSeries,  ticketValor.value);
  pushDato(clientesChartSeries, clientesValor.value);
}, 2000);

// Limpiar al desmontar la página
onUnmounted(() => clearInterval(intervalo));

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
const gaugeRendimiento = ref(55);

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