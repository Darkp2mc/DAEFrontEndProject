<template>
  <b-container>
    <h4>Atualizar o produto {{ this.nome }}</h4>
    <form @submit.prevent="update" :disabled="!isFormValid">
      <p>Escolha o Tipo do Produto</p>
      <b-form-select
        v-model="tipo"
        :state="isTipoValid"
        :options="tipoOptions"
        required
      ></b-form-select>
      <p>Escolha a Familia do Produto</p>
      <b-form-select
        v-model="familia"
        :state="isFamiliaValid"
        :options="familiaOptions"
        required
      ></b-form-select>
      <p>Escolha o E do produto</p>
      <b-input
        v-model="e"
        :state="isEValid"
        type="number"
        step=".001"
        required
      />
      <p>Escolha o N do produto</p>
      <b-input
        v-model="n"
        :state="isNValid"
        type="number"
        step=".001"
        required
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <button type="reset" @click="reset">Reset</button>
      <button @click.prevent="update" :disabled="!isFormValid">Atualizar</button>
    </form>

    <nuxt-link class="btn btn-primary" :to="`/fabricantes/${fabricanteNome}`">Return</nuxt-link
    >
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      fabricanteNome: this.$auth.user.sub,
      errorMsg: false,
      tipo: this.tipo,
      tipoOptions: [
        { value: "Chapa", text: "Chapa" },
        { value: "Laje", text: "Laje" },
        { value: "Painel", text: "Painel" },
        { value: "Perfil", text: "Perfil" },
      ],
      familia: this.familia,
      familiaOptions: [
        { value: "C", text: "C" },
        { value: "Z", text: "Z" },
        { value: "Omega", text: "Omega" },
        { value: "Outro", text: "Outro" },
      ],
      e: this.e,
      n: this.n,
    };
  },
  created() {
    this.$axios.$get(`/api/produtos/${this.nome}`)
            .then((produto)=> (this.tipo = produto.tipo,
                              this.familia=produto.familia,
                              this.e= produto.e,
                              this.n=produto.n
             ))
  },
  computed: {
    nome() {
      return this.$route.params.nome;
    },
    isTipoValid() {
      if (!this.tipo) {
        return null;
      }

      return true;
    },
    isFamiliaValid() {
      if (!this.familia) {
        return null;
      }

      return true;
    },
    isEValid() {
      if (!this.e) {
        return null;
      }

      if (this.e <= 0) {
        this.errorMsg = "Insira um valor >0";
        return false;
      }

      this.errorMsg = "";
      return true;
    },
    isNValid() {
      if (!this.n) {
        return null;
      }

      if (this.n <= 0) {
        this.errorMsg = "Insira um valor >0";
        return false;
      }

      this.errorMsg = "";
      return true;
    },
    isFormValid() {
      if (!this.isTipoValid) {
        return false;
      }

      if (!this.isFamiliaValid) {
        return false;
      }

      if (!this.isEValid) {
        return false;
      }

      if (!this.isNValid) {
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
        .put(`/api/fabricantes/${this.fabricanteNome}/produtos/${this.nome}`, {
          nome: this.nome,
          tipo: this.tipo,
          familia: this.familia,
          e: this.e,
          n: this.n,
          fabricanteUsername: this.fabricanteNome,
        })
        .then(() => {
          this.$router.push("/fabricantes/" + this.fabricanteNome);
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
};
</script>