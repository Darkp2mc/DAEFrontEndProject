<template>
  <b-container>
    <h4>Cliente Details:</h4>
    <p>Username: {{ cliente.username }}</p>
    <p>Name: {{ cliente.nome }}</p>
    <p>Email: {{ cliente.email }}</p>
    <h4>Projetos:</h4>
    <b-table v-if="projetos.length" striped over :items="projetos" :fields="projetoFields">
      <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-primary" :to="`/projetos/${row.item.nome}`">
                Details
          </nuxt-link>
      </template>
    </b-table>
    <p v-else>Sem Projetos</p>
  </b-container>

</template>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vue/1.0.18/vue.min.js"></script>
<script>
export default {
  data() {
    return {
      cliente: {},
      projetoFields:[
        "nome",
        "projetistaUsername",
        "Actions"
      ]

    };
  },
  computed: {
    username() {
      return this.$route.params.username;
    },
    projetos(){
      return this.cliente.projetoDTOs || []
    }

  },
  created() {
    this.$axios
      .$get(`/api/clientes/${this.username}`)
      .then((cliente) => (this.cliente = cliente || {}))
  }
  
  
};
</script>
