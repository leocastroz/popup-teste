<template>
	<div
		class="w-1/2 w-max bg-gradient-to-r from-emerald-500 to-green-800 p-10 rounded-lg flex items-center justify-center">
		<div v-if="spinerLoading"
			class="animate-spin rounded-full h-10 w-10 border-dashed border-2 border-white border-t-5 border-b-2 bg-gradient-to-r from-green-800 to-black">
		</div>
		<div v-if="modalCupom">
			<div class="flex justify-between">
				<p class="font-black text-base text-white">{{ config.titleCupom }}</p>
				<img class="close cursor-pointer w-5" :src="config.close" alt="fechar" @click="close" />
			</div>
			<div class="grid start mt-2">
				<button @click="copiarNumero">
					<img class="w-7" @click="hiddenCut" v-if="cutCode" :src="config.cut" alt="cortar" />
					<img class="w-7" v-else-if="pastCode" :src="config.paste" alt="colar" />
				</button>
				<p class="text-start mt-2 text-white text-sm">{{ cupom }}</p>
			</div>
		</div>
	</div>
</template>

<script>
import "../assets/config/mymodal.css";

export default {
	props: {
		config: {
			type: Object,
			required: true,
		},
		spinerLoading: Boolean,
		modalCupom: Boolean,
		cupom: String,
		cutCode: Boolean,
		pastCode: Boolean,
	},
	methods: {
		close() {
			this.$emit("closeNow");
		},
		copiarNumero() {
			this.$emit("copiar-numero");
		},
		hiddenCut() {
			this.$emit("hidden-cut");
		},
	},
};
</script>
