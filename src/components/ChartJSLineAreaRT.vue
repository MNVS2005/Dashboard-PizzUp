<template>
  <div class="chart-wrapper">
    <div class="chart-header">
      <span class="chart-title">{{ title }}</span>
      <span class="chart-value">{{ lastValue }}%</span>
    </div>
    <div class="canvas-container">
      <canvas ref="canvasRef"></canvas>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
import {
  Chart, LineController, LineElement, PointElement,
  LinearScale, TimeScale, Filler, Tooltip
} from 'chart.js';
import 'chartjs-adapter-date-fns';
import StreamingPlugin from 'chartjs-plugin-streaming';

Chart.register(LineController, LineElement, PointElement, LinearScale, TimeScale, Filler, Tooltip, StreamingPlugin);

const props = withDefaults(defineProps<{
  chartType?: 'line' | 'area';
  title?: string;
  color?: string;
  min?: number;
  max?: number;
}>(), {
  chartType: 'line',
  title: 'Métrica',
  color: '#3b82f6',
  min: 0,
  max: 100
});

const canvasRef = ref<HTMLCanvasElement | null>(null);
const lastValue = ref(0);
let chart: Chart | null = null;

const isArea = computed(() => props.chartType === 'area');

onMounted(() => {
  if (!canvasRef.value) return;

  chart = new Chart(canvasRef.value, {
    type: 'line',
    data: {
      datasets: [{
        label: props.title,
        data: [],
        borderColor: props.color,
        backgroundColor: isArea.value ? `${props.color}33` : 'transparent',
        fill: isArea.value,
        borderWidth: 2,
        pointRadius: 0,
        tension: 0.4
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      animation: false,
      plugins: {
        legend: { display: false },
        tooltip: { enabled: false },
        streaming: { duration: 20000, refresh: 1000, delay: 1000,
          onRefresh(ch) {
            const val = Math.floor(Math.random() * (props.max - props.min + 1)) + props.min;
            lastValue.value = val;
            ch.data.datasets[0].data.push({ x: Date.now(), y: val } as any);
          }
        }
      },
      scales: {
        x: {
          type: 'realtime',
          realtime: { duration: 20000, refresh: 1000, delay: 1000 },
          grid: { color: '#2a2a2a' },
          ticks: { color: '#8C8C8C', maxTicksLimit: 4,
            callback: (v: any) => new Date(v).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' })
          }
        },
        y: {
          min: props.min, max: props.max,
          grid: { color: '#2a2a2a' },
          ticks: { color: '#8C8C8C', maxTicksLimit: 4 }
        }
      }
    }
  });
});

onUnmounted(() => {
  chart?.destroy();
});
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
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}
.chart-title {
  font-size: 0.85rem;
  color: #8C8C8C;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
.chart-value {
  font-size: 1.1rem;
  font-weight: 700;
  color: white;
}
.canvas-container {
  flex: 1;
  position: relative;
  min-height: 0;
}
canvas { width: 100% !important; height: 100% !important; }
</style>