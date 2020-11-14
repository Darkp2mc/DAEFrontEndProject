<template>
  <div>
    <h1>Create a new Project</h1>
    <form @submit.prevent="create" :disabled="!isFormValid">
      <b-input
        v-model.trim="nome"
        :state="isnomeValid"
        required
        placeholder="Enter the project name"
      />
      <b-input
        v-model.trim="cliente"
        :state="isClienteValid"
        required
        placeholder="Enter the Client Name"
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link :to="`/projetistas/${username}`">Return</nuxt-link>
      <button type="reset" @click="reset">RESET</button>
      <button @click.prevent="create" :disabled="!isFormValid">CREATE</button>
    </form>
  </div>
</template>
<script>
export default {
   props: [
    'username'
  ],
    data() {
    return {
      projetistaUsername: this.username,
      nome: null,
      clienteUsername: null,
      errorMsg: false,
    };
  },
  computed: {
    
    isNomeValid() {
      if (!this.nome) {
        return null;
      }
      let nomeLen = this.nome.length;
      if (nomeLen < 3 || nomeLen > 25) {
        return false;
      }
      return true;
    },

    isClienteValid(){
      if(!this.cliente){
        return null
      }

      let clienteLen = this.cliente.length
      if(clienteLen <3 || clienteLen> 25){
        return false
      }
      return true;
    },
   
   
    isFormValid() {
      
      if (!this.isNameValid) {
        return false;
      }
      if (!this.isCliente) {
        return false;
      }
      
      return true;
    },
  },
  methods: {
    reset() {
      this.errorMsg = false;
    },
    create() {
      this.$axios
        .$post("/api/projetos", {
          nome: this.nome,
          clienteUsername: this.cliente,
          projetistaUsername: this.username
          
        })
        .then(() => {
          this.$router.push("/projetistas/"+this.username);
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
}
</script>
