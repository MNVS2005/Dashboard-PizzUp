<template>
  <VEChart class="gauge-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue';
import { use } from 'echarts/core';
import VEChart from 'vue-echarts';
import { GaugeChart } from 'echarts/charts';
import { CanvasRenderer } from 'echarts/renderers';
import { TooltipComponent, TitleComponent } from 'echarts/components';

use([GaugeChart, CanvasRenderer, TooltipComponent, TitleComponent]);

const props = withDefaults(defineProps<{
  segments: { value: number; name: string; color: string; min?: number; max?: number }[];
  title?: string;
}>(), {
  title: '',
  segments: () => []
});

const options = ref({});

watchEffect(() => {
  const total = props.segments.length;
  const series = props.segments.map((seg, i) => {
    const radius = 90 - i * 22;
    const inner  = radius - 16;
    return {
      type: 'gauge',
      radius: `${radius}%`,
      startAngle: 220, endAngle: -40,
      min: seg.min ?? 0, max: seg.max ?? 100,
      center: ['50%', '58%'],
      axisLine: {
        lineStyle: {
          width: 14,
          color: [[seg.value / (seg.max ?? 100), seg.color], [1, 'rgba(255,255,255,0.05)']]
        }
      },
      pointer: { show: false },
      axisTick: { show: false },
      splitLine: { show: false },
      axisLabel: { show: false },
      detail: {
  show: i === 0,
  offsetCenter: [0, '5%'],    // número sube, deja espacio abajo
  fontSize: 26,
  fontWeight: 'bold',
  color: '#fff',
  formatter: () => `${props.segments[0]?.value ?? 0}`
},
title: {
  show: true,
  offsetCenter: [0, `${38 + i * 28}%`],  // 38% y 66%, bien separados
  color: seg.color,
  fontSize: 11,
  fontWeight: '500'
},
      data: [{ value: seg.value, name: seg.name }]
    };
  });

  options.value = {
    backgroundColor: 'transparent',
    tooltip: { trigger: 'item', formatter: '{b}: {c}' },
    series
  };
});
</script>

<style scoped>
.gauge-chart {
  width: 100%;
  height: 100%;
  min-height: 180px;
}
</style>