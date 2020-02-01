<template>
  <div id="app">
		<Ligacoes
			:ligacoes="ligacoes"
			@excluir="excluir"
			@editar="editar"
		/>
		<button @click="() => mostraFormulario = !mostraFormulario">Motrar formulário</button>
		<FormLigacao
			v-if="mostraFormulario"
			:ligacao="ligacao"
			:ligacoes="ligacoes"
			@setar-lista="setarListar"
			@inserir-lista="inserirLista"
			@atualizar-lista="atualizarLista"
		/>
  </div>
</template>

<script>

import Ligacoes from './components/Ligacoes'
import FormLigacao from './components/FormLigacao'

export default {
	components: {
		Ligacoes,
		FormLigacao,
	},
	data() {
		return {
			ligacao:'',
			mostraFormulario:false,
			ligacoes: [
				{
					id: 2424,
					solicitante: "Germano",
					solicitado: "Luís TI Colcci",
					urgente: false,
				},
				{
					id: 2425,
					solicitante: "Márcio",
					solicitado: "Fernando TI Colcci",
					urgente: true,
				},
			],
		};
	},
	methods: {
		setarListar(valor) {
			this.ligacoes = valor
		},
		inserirLista(ligacao) {
			this.ligacoes.push(ligacao)
		},
		excluir(indice) {
			this.ligacoes.splice(indice,1);
			console.log(indice)
		},
		editar(indice) {
			this.ligacao = this.ligacoes[indice];
			this.mostraFormulario = false;

			this.$nextTick(() => {
				this.mostraFormulario = true;
			});
			
			console.log(indice)
		},
		atualizarLista(ligacao) {
			const indice = this.ligacoes.findIndex((lig) => lig.id === ligacao.id);
			
			this.ligacoes.splice(indice, 1, ligacao);

			this.mostraFormulario = false;
			this.ligacao = '';
		},
	}
};
</script>

<style>
</style>
