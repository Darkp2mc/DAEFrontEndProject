<template>
    <b-container>
        <h4>Detalhes do Projeto</h4>
        <p>Nome: {{projeto.nome}}</p>
        <p>Projetista: {{projeto.projetistaUsername}}</p>
        <p>Cliente: {{projeto.clienteUsername}}</p>
        <p>Comentarios: {{projeto.comentarios}}</p>
        <h4>Estruturas</h4>
        <b-table v-if="estruturas.length" striped over :items="estruturas" :fields="estruturasFields">

        </b-table>
        <p v-else> Sem Estruturas</p>
        <b-button variant="warning" :to="`/projetistas/${projeto.projetistaUsername}`">Back</b-button>

    </b-container>
</template>

<script>
export default {
    data() {
    return {
      projeto: {},
      estruturasFields : [
          "Nome",
          "Produto",
          "Projeto",
          "Dimensoes"


      ]
    }
  },

  computed: {
      nome() {
          return this.$route.params.nome
      },
      estruturas() {
          return this.projeto.estruturas || []

      }
  },

  created() {
      this.$axios 
        .$get(`/api/projetos/${this.nome}`)
        .then((projeto)=> (this.projeto = projeto || {}))
  }
    
}
</script>