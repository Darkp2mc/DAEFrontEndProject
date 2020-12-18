<template>
  <b-container>
    <h4>Criar novo Produto</h4>
    <form @submit.prevent="create" :disabled="!isFormValid">
      <b-input
        v-model.trim="nome"
        :state="isNomeValid"
        required
        placeholder="Insira o nome do produto"
      />
      <b-form-select
        v-model.trim="tipo"
        :state="isTipoValid"
        :options="tipoOptions"
        required
      ></b-form-select>
      <b-form-select
        v-model.trim="familia"
        :state="isFamiliaValid"
        :options="familiaOptions"
        required
      ></b-form-select>
      <b-input
        v-model.trim="e"
        :state="isEValid"
        required
        placeholder="Insira o E do produto"
      />
      <b-input
        v-model.trim="N"
        :state="isNValid"
        required
        placeholder="Insira o N do produto"
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <button type="reset" @click="reset">Reset</button>
      <button @click.prevent="create" :disabled="!isFormValid">Criar</button>
    </form>

    <nuxt-link class="btn btn-primary" :to="`/fabricantes/${fabricanteNome}`"
      >Return</nuxt-link
    >
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      fabricanteNome: this.$auth.user.sub,
      errorMsg: false,
      nome: null,
      tipo: null,
      tipoOptions: [
        { value: null, text: "Escolha o tipo de Produto" },
        { value: "Chapa", text: "Chapa" },
        { value: "Laje", text: "Laje" },
        { value: "Painel", text: "Painel" },
        { value: "Perfil", text: "Perfil" },
      ],
      familia: null,
      familiaOptions: [
        { value: null, text: "Escolha a Familia" },
        { value: "C", text: "C" },
        { value: "Z", text: "Z" },
        { value: "Omega", text: "Omega" },
        { value: "Outro", text: "Outro" },
      ],
      e: '',
      N: '',
    };
  },
  computed: {
    isNomeValid() {
      if (!this.nome) {
        return null;
      }
      let nomeLen = this.nome.length;
      if (nomeLen < 3 || nomeLen > 25) {
        this.errorMsg = "Insira um nome entre 3 e 25 caraters";
        return false;
      }
      this.errorMsg = "";
      return true;
    },
    isTipoValid() {  
      if(!this.tipo){
        return null
      }
      
      return true
    },
    isFamiliaValid(){
        if(!this.familia){
            return null
        }

        return true
    },
    isEValid(){
        if(!this.e){
            return null
        }

        if(this.e<=0){
            this.errorMsg ="Insira um valor >0"
            return false
        }

        this.errorMsg= ''
        return true
    },
    isNValid() {
       if(!this.N){
            return null
        }

        if(this.N<=0){
            this.errorMsg ="Insira um valor >0"
            return false
        }

        this.errorMsg= ''
        return true 
    },
    isFormValid(){
        if(!this.isNomeValid){
            return false
        }

        if(!this.isTipoValid){
            return false
        }

        if(!this.isFamiliaValid){
            return false
        }

        if(!this.isEValid) {
            return false
        }

        if(!this.isNValid) {
            return false
        }

        return true
    }
  },

  methods: {
    reset() {
      this.errorMsg = false;
    },
    create() {
      this.$axios
        .post("/api/produtos", {
            nome: this.nome,
            tipo: this.tipo,
            familia: this.familia,
            e: this.e,
            n: this.N,
            fabricanteUsername: this.fabricanteNome
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