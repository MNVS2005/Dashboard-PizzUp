<template>
  <div class="box-mixedChart">
    <VueApexCharts
      type="line"
      height="100%"
      :options="options"
      :series="series"
    />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts';
import { withDefaults, defineProps } from 'vue';
import type { ApexOptions, ApexAxisChartSeries } from 'apexcharts';

/* =========================
   📥 PROPS
========================= */
const props = withDefaults(
  defineProps<{
    series: ApexAxisChartSeries;
    options?: ApexOptions;
  }>(),
  {
    options: () => ({
      chart: {
        type: 'line',
        stacked: false,
        background: 'transparent',
        toolbar: { show: false }
      },

      theme: {
        mode: 'dark',
        palette: 'palette1'
      },

      title: {
        text: 'Mixed Chart',
        align: 'left',
        style: {
          fontSize: '16px',
          fontWeight: 'bold',
          color: '#8C8C8C'
        }
      },

      stroke: {
        width: [0, 2, 4],
        curve: 'smooth'
      },

      plotOptions: {
        bar: {
          columnWidth: '50%'
        }
      },

      fill: {
        opacity: [0.85, 0.25, 1],
        gradient: {
          inverseColors: false,
          shade: 'light',
          type: 'vertical',
          opacityFrom: 0.85,
          opacityTo: 0.55,
          stops: [0, 100, 100, 100]
        }
      },

      labels: [
        '2023-01-01', '2023-02-01', '2023-03-01',
        '2023-04-01', '2023-05-01', '2023-06-01',
        '2023-07-01', '2023-08-01', '2023-09-01',
        '2023-10-01', '2023-11-01'
      ],

      markers: {
        size: 0
      },

      xaxis: {
        type: 'datetime',
        labels: {
          style: { colors: '#8C8C8C' }
        }
      },

      yaxis: {
        labels: {
          style: { colors: '#8C8C8C' }
        },
        title: {
          style: {
            color: '#8C8C8C',
            fontWeight: 'normal'
          }
        },
        tooltip: {
          enabled: true
        }
      },

      grid: {
        show: true,
        borderColor: '#555'
      },

      legend: {
        position: 'top',
        labels: {
          colors: '#8C8C8C'
        }
      },

      tooltip: {
        shared: true,
        theme: 'dark',
        intersect: false,
        y: {
          formatter: (y: number | null) =>
            y !== null ? `${y.toFixed(0)} puntos` : ''
        }
      }
    })
  }
);
</script>

<style scoped>
.box-mixedChart {
  width: 100%;
  height: 100%;
  min-height: 250px;
}
</style>