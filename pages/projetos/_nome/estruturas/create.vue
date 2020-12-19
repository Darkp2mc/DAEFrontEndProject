<template>
  <b-container>
    <h4>Criar uma Nova Estrutura</h4>
    <form @submit.prevent="create" :disabled="!isFormValid">
        <b-input
            v-model="nome"
            :state="isNomeValid"
            required
            placeholder="Insira o nome da estrutura"
        />
        <b-form-select 
            v-model="tipoDeProduto"
            :state="isTipoValid" 
            :options="options"
            required>
        </b-form-select>
        <b-input
        v-model="numeroDeVaos"
        :state="isNumVaosValid"
        type="number"
        step=".001"
        required
        placeholder="Insira o numero de vaos"
        />
        <b-input
        v-model="comprimentoDaVao"
        :state="isCompValid"
        type="number"
        step=".001"
        required
        placeholder="Insira o comprimento do vao"
        />
        <b-form-select 
            v-if="tipoDeProduto!='Lage'"
            v-model="aplicacao"
            :state="isAppValid" 
            :options="optionsApp"
            required>
        </b-form-select>
        <b-input
        v-if="tipoDeProduto=='Lage'"
        v-model.trim="alturaDaLage"
        :state="isAlturaValid"
        required
        placeholder="Insira altura da lage"
        />
        <b-input
        v-model="sobrecarga"
        :state="isSobrecargaValid"
        type="number"
        step=".001"
        required
        placeholder="Insira  sobrecarga da estrutura"
        />

        <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
        
      <button type="reset" @click="reset">Reset</button>
      <button @click.prevent="create" :disabled="!isFormValid">Criar</button>
    </form>

    <nuxt-link
      class="btn btn-primary"
      :to="`/projetos/${projetoNome}`"
      >Voltar</nuxt-link
    >
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      nome: null,
      projetoNome: this.$route.params.nome,
      errorMsg: false,
      tipoDeProduto: null,
      options:[
          { value: null, text: 'Escolha o tipo de Produto' },
          { value: 'Chapa', text: 'Chapa' },
          { value: 'Lage', text: 'Lage' },
          { value: 'Painel', text: 'Painel' },
          { value: 'Perfil', text: 'Perfil' }
      ],
      aplicacao: null,
      optionsApp:[
         { value: null, text: 'Escolha o tipo de Aplicaçao' },
          { value: 'Cobertura', text: 'Cobertura' },
          { value: 'Fachada', text: 'Fachada' },
          { value: 'Suporte', text: 'Suporte' }
      ],
      numeroDeVaos: null,
      comprimentoDaVao: null,
      alturaDaLage: null,
      sobrecarga: null,
      estado: null    
    }
  },
  computed: {
      isNomeValid() {
      if (!this.nome) {
        return null
      }
      let nomeLen = this.nome.length;
      if (nomeLen < 3 || nomeLen > 25) {
        this.errorMsg= "Insira um nome entre 3 e 25 caraters"
        return false;
      }
      this.errorMsg= ''
      return true;
    },
    isTipoValid(){
      if(!this.tipoDeProduto){
        return null
      }
      
      return true
    },
    isNumVaosValid(){
      if(!this.numeroDeVaos){
        return null
      }
      if(this.numeroDeVaos<=0){
        this.errorMsg ="Insira um valor >0"
        return false
      }
      this.errorMsg= ''
      return true
    },
    isCompValid(){
      if(!this.comprimentoDaVao){
        return null
      }
      if(this.comprimentoDaVao<=0){
       this.errorMsg ="Insira um valor >0"
        return false
      }
      this.errorMsg= ''
      return true
    },
    isAppValid(){
      if(this.tipoDeProduto!='Lage'){
        if(!this.aplicacao){
          return null
        }
        return true
      }
      return true
    },
    isAlturaValid(){
      if(this.tipoDeProduto=='Lage'){
        if(!this.alturaDaLage){
          return null

        }
        if(this.alturaDaLage<=0){
          this.errorMsg ="Insira um valor >0"
          return false
        }
        this.errorMsg= ''
        return true
      }
      return true
    },
    isSobrecargaValid(){
      if(!this.sobrecarga){
        return null
      }

      if(this.sobrecarga<=0){
        this.errorMsg ="Insira um valor >0"
        return false
      }
      this.errorMsg= ''
      return true
    },
    isFormValid(){
      if(!this.isNomeValid){
        return false

      }if(!this.isTipoValid){
        return false
      }

      if(!this.isNumVaosValid){
        return false
      }

      if(!this.isCompValid){
        return false
      }

      if(!this.isAppValid){
        return false
      }

      if(!this.isAlturaValid){
        return false
      }

      if(!this.isSobrecargaValid){
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
        .post("/api/estruturas", {
            nome: this.nome,
            projetoNome: this.projetoNome,
            tipoDeProduto: this.tipoDeProduto,
            numeroDeVaos: this.numeroDeVaos,
            comprimentoDaVao: this.comprimentoDaVao,
            aplicacao: this.aplicacao,
            alturaDaLage: this.alturaDaLage,
            sobrecarga: this.sobrecarga,
            estado: 0
        })
        .then(()=> {
         this.$router.push("/projetos/"+this.projetoNome);
          
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        })
    }
  }
};
</script>