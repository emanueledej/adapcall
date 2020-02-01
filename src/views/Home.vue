<template>
  <div class="home">
    <Ligacoes
      :ligacoes="ligacoes"
      :is-loading="isLoading"
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

import Ligacoes from '../components/Ligacoes';
import FormLigacao from '../components/FormLigacao';


export default {
  name: 'home',
  components: {
    Ligacoes,
    FormLigacao,
  },
  data() {
    return {
      ligacao: '',
      mostraFormulario: false,
      isLoading:false,
      ligacoes: [
        {
          id: 2424,
          solicitante: 'Germano',
          solicitado: 'Luís TI Colcci',
          urgente: false,
        },
        {
          id: 2425,
          solicitante: 'Márcio',
          solicitado: 'Fernando TI Colcci',
          urgente: true,
        },
      ],
    };
  },
  async mounted(){
    try {
      this.isLoading = true;
      const res = await axios.get('https://5e3589a4f7e55d0014ad4dc7.mockapi.io/ligacoes');

      if (Array.isArray(res.data)) this.ligacoes = [...this.ligacoes,...res.data];
      
      console.log('res', res); 
    } catch (err) {
      console.log('err', err);
    } finally {
      this.isLoading = false;
    }
    
  },
  methods: {
    setarListar(valor) {
      this.ligacoes = valor;
    },
    inserirLista(ligacao) {
      this.ligacoes.push(ligacao);
    },
    excluir(indice) {
      this.ligacoes.splice(indice, 1);
      console.log(indice);
    },
    editar(indice) {
      this.ligacao = this.ligacoes[indice];
      this.mostraFormulario = false;

      this.$nextTick(() => {
        this.mostraFormulario = true;
      });

      console.log(indice);
    },
    atualizarLista(ligacao) {
      const indice = this.ligacoes.findIndex(lig => lig.id === ligacao.id);

      this.ligacoes.splice(indice, 1, ligacao);

      this.mostraFormulario = false;
      this.ligacao = '';
    },
  },
};
</script>
