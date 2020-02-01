<template>
	<form @submit.prevent="salvar">
		<div v-if="editando">Você está editando a ligação: {{formLigacao.id}}</div>
		<fieldset>
			<legend>Criar Ligação</legend>
			{{ formLigacao }}
			<div>
				Solicitante:
				<input type="text"
					v-model="formLigacao.solicitante"
					required
				>
			</div>
			<div>
				Solicitado:
				<input type="text"
					v-model="formLigacao.solicitado"
					required
				>
			</div>
			<div>
				Urgente:
				<input type="checkbox"
					v-model="formLigacao.urgente"
				>
			</div>
			<button type="submit"> {{ editando ? 'Salvar' : 'Criar Ligação' }} </button>
			<button type="button" @click="apagarLista"> Apagar Lista </button>
		</fieldset>
	</form>
</template>

<script>

const novaLigacao = {
  id: '',
  solicitante: '',
  solicitado: '',
  urgente: false,
};

export default {
  props: ['ligacoes', 'ligacao'],
  data() {
    return {
      formLigacao: { ...novaLigacao },
      editando: false,
    };
  },
  mounted() {
    if (this.ligacao) {
      this.formLigacao = { ...this.ligacao };
      this.editando = true;
    } else {
      this.editando = false;
    }
  },
  methods: {
    salvar() {
      // cópia do objeto de primeiro nível ---- todo estudar referências e cópias de objetos em js
      const ligacaoAInserir = {
        ...this.formLigacao,
      };

      if (!this.editando) {
        ligacaoAInserir.id = this.ligacoes.length + 1;
        this.$emit('inserir-lista', ligacaoAInserir);
      } else {
        this.$emit('atualizar-lista', ligacaoAInserir);
      }

      this.formLigacao = { ...novaLigacao };
    },
    apagarLista() {
      this.$emit('setar-lista', []);
    },
  },
};
</script>

<style>

</style>
