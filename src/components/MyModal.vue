<template>
	<div class="modal-success  w-1/2 w-max p-10 rounded-lg flex items-center justify-center">
		<div v-if="spinerLoading"
			class="animate-spin rounded-full h-10 w-10 border-dashed border-2 border-white border-t-5 border-b-2 bg-gradient-to-r from-blue-300 to-blue-500">
		</div>
		<div v-if="modalCupom">
			<div class="flex justify-between">
				<p class="font-black text-base text-white">{{ config.cupomModal.title }}</p>
				<img class="close cursor-pointer w-5" :src="config.cupomModal.closeModal" alt="fechar" @click="close" />
			</div>
			<div class="grid start mt-2">
				<button @click="copiarNumero">
					<img class="w-7" @click="hiddenCut" v-if="cutCode" :src="config.cupomModal.cutCode" alt="cortar" />
					<img class="w-7" v-else-if="pastCode" :src="config.cupomModal.pasteCode" alt="colar" />
				</button>
				<p class="text-start mt-2 text-blue-200 text-sm">{{ cupom }}</p>
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
