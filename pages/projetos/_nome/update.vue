<template>
  <div>
    <h1>Update a Project</h1>
    <form @submit.prevent="update" :disabled="!isFormValid">
      <b-input
        v-model="clienteUsername"
        :state="isClienteValid"
        required
        
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>

      <nuxt-link class="btn btn-primary" :to="`/projetos/${nome}`">Return</nuxt-link>

      <button type="reset" @click="reset">RESET</button>

      <button @click.prevent="update" :disabled="!isFormValid">Update</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      projeto: {},
      clienteUsername: this.projetistaUsername,
      projetistaUsername: this.$auth.user.sub,
      errorMsg: false,
    };
  },

  created() {
    this.$axios.$get(`/api/projetos/${this.nome}`).then((projeto) => {
      this.clienteUsername = projeto.clienteUsername
    });
  },
  computed: {
    nome() {
      return this.$route.params.nome;
    },
    isClienteValid() {
      if (!this.clienteUsername) {
        return null;
      }

      let clienteLen = this.clienteUsername.length;
      if (clienteLen < 3 || clienteLen > 25) {
        return false;
      }
      return true;
    },
    isFormValid() {
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
    update() {
      this.$axios
        .$put(
          "/api/projetistas/" + this.projetistaUsername + "/projetos/" + this.nome,
          {
            nome: this.nome,
            projetistaUsername: this.projetistaUsername,
            clienteUsername: this.clienteUsername,
          }
        )
        .then(() => {
          this.$router.push("/projetos/" + this.nome);
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
};
</script>