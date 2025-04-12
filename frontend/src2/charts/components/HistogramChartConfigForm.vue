<script setup lang="ts">
import { ref, watch } from 'vue'
import type { HistogramChartConfig } from '../../types/chart.types'
import type { ColumnOption } from '../../types/query.types'
import MeasurePicker from './MeasurePicker.vue'

const props = defineProps<{
	modelValue: HistogramChartConfig
	columnOptions: ColumnOption[]
}>()

const emit = defineEmits<{
	(e: 'update:modelValue', value: HistogramChartConfig): void
}>()

const config = ref<HistogramChartConfig>({
	value_column: props.modelValue?.value_column || {},
	classInterval: props.modelValue?.classInterval || 10,
})

// Emit updates when config changes
watch(config, (newConfig: HistogramChartConfig) => {
	emit('update:modelValue', newConfig)
}, { deep: true })

// Sync external prop changes into local config
watch(() => props.modelValue, (newVal: HistogramChartConfig) => {
	config.value = {
		value_column: newVal.value_column || {},
		classInterval: newVal.classInterval || 10,
	}
}, { deep: true })
</script>


<template>
	<div class="space-y-4">
		<MeasurePicker
			label="Numeric Column"
			v-model="config.value_column"
			:column-options="props.columnOptions"
		/>

		<div>
			<label class="text-sm font-medium text-gray-700">Bin Size</label>
			<input
				type="number"
				class="frappe-control"
				v-model.number="config.classInterval"
				min="1"
				placeholder="Enter bin size"
			/>
		</div>
	</div>
</template>

<style scoped>
.frappe-control {
	width: 100%;
	padding: 0.5rem;
	border: 1px solid #d1d5db;
	border-radius: 0.375rem;
	font-size: 0.875rem;
}
</style>
