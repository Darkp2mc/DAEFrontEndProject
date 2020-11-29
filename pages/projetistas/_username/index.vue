<template>
  <b-container>
    <h4>Projetista Details:</h4>
    <p>Username: {{ projetista.username }}</p>
    <p>Name: {{ projetista.nome }}</p>
    <p>Email: {{ projetista.email }}</p>
    <h4>Projetos:</h4>
    <b-table v-if="projetos.length" striped over :items="projetos" :fields="projetoFields">
      <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-primary" :to="`/projetos/${row.item.nome}`">
                Details
          </nuxt-link>
          <b-button variant="danger" v-on:click="deleteProjeto(row.item.nome)">DELETE</b-button>
      </template>
    </b-table>
    <p v-else>Sem Projetos</p>

    <nuxt-link class="btn btn-success"  :to="`/projetos/create`">Create New Project</nuxt-link>

  </b-container>

</template>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vue/1.0.18/vue.min.js"></script>
<script>
export default {
  data() {
    return {
      projetista: {},
      projetoFields:[
        "nome",
        "clienteUsername",
        "Actions"
      ]

    };
  },
  computed: {
    username() {
      return this.$route.params.username;
    },
    projetos(){
      return this.projetista.projetos || []
    }

  },
  created() {
    this.$axios
      .$get(`/api/projetistas/${this.username}`)
      .then((projetista) => (this.projetista = projetista || {}))


  },
  methods:{
    deleteProjeto(projetoNome){
      this.$axios.
      $delete(`/api/projetistas/${this.username}/projetos/${projetoNome}`)
      .then(()=> {
        window.location.reload()
      })
      .catch(errors=>
        console.log(errors)
      )
    }
  }
};
</script>
