<template>
  <div>
    <h1>Create a new Project</h1>
    <form @submit.prevent="create" :disabled="!isFormValid">
      <b-input
        v-model.trim="nome"
        :state="isNomeValid"
        required
        placeholder="Enter the project name"
      />
      <b-input
        v-model.trim="clienteUsername"
        :state="isClienteValid"
        required
        placeholder="Enter the Client UserName"
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>

      <nuxt-link class="btn btn-primary" :to="`/projetistas/${projetistaUsername}`">Return</nuxt-link>

      <button type="reset" @click="reset">RESET</button>

      <button @click.prevent="create" :disabled="!isFormValid">CREATE</button>
    </form>
  </div>
</template>
<script>
export default {
   
    data() {
    return {
      nome: null,
      clienteUsername: null,
      projetistaUsername: this.$auth.user.sub,
      errorMsg: false,
    };
  },
  computed: {
    
    isNomeValid() {
      if (!this.nome) {
        return null
      }
      let nomeLen = this.nome.length;
      if (nomeLen < 3 || nomeLen > 25) {
        return false;
      }
      return true;
    },

    isClienteValid(){
      if(!this.clienteUsername){
        return null
      }

      let clienteLen = this.clienteUsername.length
      if(clienteLen <3 || clienteLen> 25){
        return false
      }
      return true;
    },
   
   
    isFormValid() {
      
      if (!this.isNomeValid) {
        return false;
      }
      if (!this.isClienteValid) {
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
          clienteUsername: this.clienteUsername,
          projetistaUsername: this.projetistaUsername
        })
        .then(() => {
          this.$router.push("/projetistas/"+this.projetistaUsername);
          
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
}
</script>
