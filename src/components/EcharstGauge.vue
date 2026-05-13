<template>
  <VEChart class="gauge-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue';

// ECharts
import { use } from "echarts/core";
import VEChart from "vue-echarts";
import { GaugeChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";
import { TooltipComponent } from "echarts/components";

// ✅ Registrar módulos
use([GaugeChart, CanvasRenderer, TooltipComponent]);

/* =========================
   📥 PROPS
========================= */
const props = defineProps<{
  value: number;
  title?: string;
}>();

/* =========================
   ⚙️ OPTIONS
========================= */
const options = ref({});

watchEffect(() => {
  options.value = {
    series: [
      {
        type: 'gauge',
        center: ['50%', '55%'],
        radius: '95%',

        min: 0,
        max: 100,

        axisLine: {
          lineStyle: {
            width: 20,
            color: [
              [0.2, "#ff4d4d"],   // rojo
              [0.69, "#ffa500"],  // naranja
              [1, "#4caf50"],     // verde
            ]
          }
        },

        pointer: {
          length: '60%',
          itemStyle: {
            color: 'auto'
          }
        },

        axisTick: {
          distance: -20,
          length: 4,
          lineStyle: {
            color: '#fff',
            width: 1
          }
        },

        splitLine: {
          distance: -20,
          length: 20,
          lineStyle: {
            color: '#fff',
            width: 2
          }
        },

        axisLabel: {
          color: '#ccc',
          distance: 28,
          fontSize: 14,
          formatter: (value: number) =>
            value % 20 === 0 ? value : ''
        },

        detail: {
          valueAnimation: true,
          formatter: '{value}%',
          color: '#fff',
          fontSize: 28,
          offsetCenter: [0, '20%']
        },

        data: [
          {
            value: props.value,
            name: props.title ?? 'KPI',
            title: {
              color: '#8C8C8C',
              fontSize: 14,
              offsetCenter: [0, "75%"]
            }
          }
        ]
      }
    ]
  };
});
</script>

<style scoped>
.gauge-chart {
  width: 100%;
  height: 100%;
  min-height: 300px;
}
</style>