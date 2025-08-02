<template>
  <RadarChart :data="chartData" />
</template>

<script>
import { RadarChart } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Filler,
  Legend,
  LineElement,
  PointElement,
  RadialLinearScale,
  Tooltip
} from 'chart.js'

ChartJS.register(
  Filler,
  Legend,
  LineElement,
  PointElement,
  RadialLinearScale,
  Tooltip
)

export default {
  name: 'PokemonRadarChart',
  components: { RadarChart },
  props: {
    chartData: {
      type: Object,
      required: true
    }
  }
}
</script>