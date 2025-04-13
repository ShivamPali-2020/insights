<script setup lang="ts">
import { onMounted, watch, ref } from 'vue'
// import Plotly from 'plotly.js'
import { PlotData } from 'plotly.js'
import type { HistogramChartConfig } from '../../types/chart.types'
import type { QueryResult } from '../../types/query.types'

const props = defineProps<{
	config: HistogramChartConfig
	result: QueryResult
}>()

const plotRef = ref<HTMLElement | null>(null)

const renderChart = () => {
	if (!plotRef.value || !props.config || !props.result) return

	// const colName = props.config.value_column?.column_name
	// if (!colName) return

	// const colIndex = props.result.columns.findIndex((col) => col.name === colName)
	// if (colIndex === -1) return

	// // Extract raw numeric values from query result rows
	// const rawValues =
	// 	props.result.formattedRows
	// 		?.map((row) => row[colIndex])
	// 		?.filter((val) => typeof val === 'number') || []

	// Histogram Data
	const rawValues: number[] = Array.from({ length: 500 }, () => Math.random() * 100)

	const trace: Partial<PlotData> = {
		x: rawValues,
		type: 'histogram',
		xbins: {
			start: Math.min(...rawValues),
			end: Math.max(...rawValues),
			size: props.config.classInterval || 10,
		},
		marker: {
			color: '#1f77b4',
		},
	}

	const layout = {
		title: `Histogram of Sachin & Shivam`,
		xaxis: { title: 'Histogram of Sachin & Shivam' },
		yaxis: { title: 'Frequency' },
		margin: { t: 50, l: 50, r: 30, b: 50 },
		autosize: true,
		transition: {
			duration: 500,
			easing: 'cubic-in-out' as const,
		},
	}

	// Plotly.react(plotRef.value, [trace], layout, { responsive: true })
	console.log('Histogram', [trace], layout, { responsive: true })
}

onMounted(renderChart)
watch(() => [props.config, props.result], renderChart, { deep: true })
</script>

<template>
	<div ref="plotRef" class="h-full w-full sachin-shivam-histogram"></div>
</template>

<style scoped>
/* Ensures the chart fills its container */
div {
	min-height: 300px;
}
</style>
