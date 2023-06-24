<template>
	<div>
		<div class="pt-3">
			<label :for="inputId" class="text-violet-300 pr-2 text-xs">{{ label }}</label>
			<input :id="inputId" type="checkbox" :checked="checked" @change="handleChange" />
		</div>
	</div>
</template>
	
<script>
export default {
	props: {
		inputId: {
			type: String,
			required: true,
		},
		label: {
			type: String,
			required: true,
		},
		checked: {
			type: Boolean,
			required: true,
		}
	},
	methods: {
		handleChange(event) {
			this.$emit('update:checked', event.target.checked)
		}
	}
}
</script>
	