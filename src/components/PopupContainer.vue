<template>
	<div class="description mx-auto my-10 rounded-xl p-5 w-80 border-solid border-2 border-cyan-200">
		<h2 class="font-black uppercase text-black w-full text-center py-3 text-sky-200">{{ title }}</h2>
		<p class="text-center text-sm text-zinc-100">{{caption}}</p>
	</div>
</template>

<script>
export default {
	data() {
		return {
			title: "Bem-vindo à Popup",
			caption: "Esse é um componente de popup genérico, que pode ser usado para exibir qualquer onteúdo."
		}
	}
}
</script>
