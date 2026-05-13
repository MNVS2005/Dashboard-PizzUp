<template>
  <div class="chart-wrapper">
    <div class="chart-header">
      <span class="chart-title">{{ title }}</span>
      <span class="chart-badge" :class="isAboveTarget ? 'badge-green' : 'badge-red'">
        {{ isAboveTarget ? '▲' : '▼' }} KPI {{ kpiTarget }}
      </span>
    </div>
    <vapexChart
      class="chart"
      type="line"
      :height="'85%'"
      :options="chartOptions"
      :series="series"
    />
  </div>
</template>

<script setup lang="ts">
import vapexChart from 'vue3-apexcharts';
import { computed } from 'vue';

const props = withDefaults(defineProps<{
  series: { name: string; data: { x: number; y: number }[] }[];
  title?: string;
  kpiTarget?: number;
  color?: string;
}>(), {
  title: 'Tiempo real',
  kpiTarget: 70,
  color: '#3b82f6'
});

const lastValue = computed(() => {
  const d = props.series[0]?.data;
  return d?.length ? d[d.length - 1].y : 0;
});

const isAboveTarget = computed(() => lastValue.value >= props.kpiTarget);

const chartOptions = computed(() => ({
  chart: {
    id: 'realtime',
    type: 'line',
    animations: { enabled: true, easing: 'linear', dynamicAnimation: { speed: 900 } },
    toolbar: { show: false },
    background: 'transparent',
    zoom: { enabled: false }
  },
  theme: { mode: 'dark' },
  stroke: { curve: 'smooth', width: 2 },
  colors: [props.color],
  xaxis: {
    type: 'datetime',
    labels: { style: { colors: '#8C8C8C' } },
    axisBorder: { show: false },
    axisTicks: { show: false }
  },
  yaxis: {
    min: 0, max: 100,
    labels: { style: { colors: '#8C8C8C' } },
    tickAmount: 4
  },
  annotations: {
    yaxis: [{
      y: props.kpiTarget,
      borderColor: '#f97316',
      strokeDashArray: 4,
      label: {
        text: `KPI ${props.kpiTarget}`,
        style: { color: '#f97316', background: 'transparent', fontSize: '11px' }
      }
    }]
  },
  grid: { borderColor: '#2a2a2a', strokeDashArray: 3 },
  tooltip: { theme: 'dark', x: { format: 'HH:mm:ss' } },
  legend: { show: false }
}));
</script>

<style scoped>
.chart-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  padding: 12px;
}
.chart-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 4px;
}
.chart-title {
  font-size: 0.85rem;
  color: #8C8C8C;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
.chart-badge {
  font-size: 0.75rem;
  padding: 2px 8px;
  border-radius: 4px;
  font-weight: 600;
}
.badge-green { background: rgba(16,185,129,0.15); color: #10b981; }
.badge-red   { background: rgba(239,68,68,0.15);  color: #ef4444; }
.chart { flex: 1; }
</style>