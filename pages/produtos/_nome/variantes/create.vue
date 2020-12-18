<template>
  <b-container>
    <h3>Criar Variante do produto {{ nomeProduto }}</h3>
    <b-form @submit.prevent="create" :disabled="!isFormValid">
      <b-input
        v-model.trim="codigo"
        :state="isCodigoValid"
        required
        placeholder="Insira o codigo da variante"
      />
      <b-input
        v-model.trim="nome"
        :state="isNomeValid"
        required
        placeholder="Insira o nome da variante"
      />
      <b-input
        v-model.trim="weff_p"
        :state="isWeff_pValid"
        required
        placeholder="Insira o weff_p do produto"
      />
      <b-input
        v-model.trim="weff_n"
        :state="isWeff_nValid"
        required
        placeholder="Insira o weff_n do produto"
      />
      <b-input
        v-model.trim="ar"
        :state="isArValid"
        required
        placeholder="Insira o ar do produto"
      />
      <b-input
        v-model.trim="sigmaC"
        :state="isSigmaCValid"
        required
        placeholder="Insira o sigmaC do produto"
      />
      <b-input
        v-model.trim="h_mm"
        :state="isH_mmValid"
        required
        placeholder="Insira o h_mm do produto"
      />
      <b-input
        v-model.trim="b_mm"
        :state="isB_mmValid"
        required
        placeholder="Insira o B_mm do produto"
      />
      <b-input
        v-model.trim="c_mm"
        :state="isC_mmValid"
        required
        placeholder="Insira o C_mm do produto"
      />
      <b-input
        v-model.trim="t_mm"
        :state="isT_mmValid"
        required
        placeholder="Insira o t_mm do produto"
      />
      <b-input
        v-model.trim="a_mm"
        :state="isA_mmValid"
        required
        placeholder="Insira o a_mm do produto"
      />
      <b-input
        v-model.trim="p_kg_m"
        :state="isP_kg_mValid"
        required
        placeholder="Insira o p_kg_m do produto"
      />
      <b-input
        v-model.trim="yg_mm"
        :state="isYg_mmValid"
        required
        placeholder="Insira o yg_mm do produto"
      />
      <b-input
        v-model.trim="zg_mm"
        :state="isZg_mmValid"
        required
        placeholder="Insira o zg_mm do produto"
      />
      <b-input
        v-model.trim="ly_mm"
        :state="isLy_mmValid"
        required
        placeholder="Insira o Ly_mm do produto"
      />
      <b-input
        v-model.trim="wy_mm"
        :state="isWy_mmValid"
        required
        placeholder="Insira o Wy_mm do produto"
      />
      <b-input
        v-model.trim="lz_mm"
        :state="isLz_mmValid"
        required
        placeholder="Insira o lz_mm do produto"
      />
      <b-input
        v-model.trim="wz_mm"
        :state="isWz_mmValid"
        required
        placeholder="Insira o wz_mm do produto"
      />
      <b-input
        v-model.trim="ys_mm"
        :state="isYs_mmValid"
        required
        placeholder="Insira o ys_mm do produto"
      />
      <b-input
        v-model.trim="zs_mm"
        :state="isZs_mmValid"
        required
        placeholder="Insira o zs_mm do produto"
      />
      <b-input
        v-model.trim="lt_mm"
        :state="isLt_mmValid"
        required
        placeholder="Insira o lt_mm do produto"
      />
      <b-input
        v-model.trim="lw_mm"
        :state="isLw_mmValid"
        required
        placeholder="Insira o lw_mm do produto"
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <button type="reset" @click="reset">Reset</button>
      <button @click.prevent="create" :disabled="!isFormValid">Criar</button>
    </b-form>
    <nuxt-link class="btn btn-primary" :to="`/produtos/${nomeProduto}`"
      >Return</nuxt-link
    >
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      nomeProduto: this.$route.params.nome,
      errorMsg: null,
      codigo: null,
      nome: null,
      weff_p: null,
      weff_n: null,
      ar: null,
      sigmaC: null,
      h_mm: null,
      b_mm: null,
      c_mm: null,
      t_mm: null,
      a_mm: null,
      p_kg_m: null,
      yg_mm: null,
      zg_mm: null,
      ly_mm: null,
      wy_mm: null,
      lz_mm: null,
      wz_mm: null,
      ys_mm: null,
      zs_mm: null,
      lt_mm: null,
      lw_mm: null,
    };
  },
  computed: {
    isCodigoValid() {
      if (!this.codigo) {
        return null;
      }
      if (this.codigo <= 0) {
        this.errorMsg = "Código tem de ser um valor >0";
        return false;
      }
      this.errorMsg = "";
      return true;
    },
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
    isWeff_pValid(){
        if (!this.weff_p) {
        this.errorMsg = "introduza valor";
        return false;
      }
      this.errorMsg = "";
      return true;

    },
    isWeff_nValid(){
        if (!this.weff_n) {
        this.errorMsg = "introduza valor";
        return false;
      }
      
      this.errorMsg = "";
      return true;

    },
    isArValid(){
        if (!this.ar) {
       this.errorMsg = "introduza valor";
        return false;
      }
      
      this.errorMsg = "";
      return true;

    },
    isSigmaCValid(){
        if (!this.sigmaC) {
        this.errorMsg = "introduza valor";
        return false;
      }
      
      this.errorMsg = "";
      return true;

    },
    isH_mmValid(){
        if (!this.h_mm) {
        this.errorMsg = "introduza valor";
        return false;
      }
      this.errorMsg = "";
      return true;
    },
    isB_mmValid(){
        if(!this.b_mm){
            this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true;
    },
    isC_mmValid(){
        if(!this.c_mm){
          this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true;
    },
    isT_mmValid(){
        if(!this.t_mm){
          this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true

    },
    isA_mmValid(){
        if(!this.a_mm){
          this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true

    },
    isP_kg_mValid(){
        if(!this.p_kg_m){
            this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isYg_mmValid(){
        if(!this.yg_mm){
         this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isZg_mmValid(){
        if(!this.zg_mm){
          this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isLy_mmValid(){
        if(!this.ly_mm){
            this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true

    },
    isWy_mmValid(){
        if(!this.wy_mm){
           this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isLz_mmValid(){
        if(!this.lz_mm){
          this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isWz_mmValid(){
        if(!this.wz_mm){
           this.errorMsg = "introduza valor";
        return false;
        }
       
      this.errorMsg = "";
      return true
    },
    isYs_mmValid(){
        if(!this.ys_mm){
           this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isZs_mmValid(){
        if(!this.zs_mm){
           this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isLt_mmValid(){
        if(!this.lt_mm){
           this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isLw_mmValid(){
        if(!this.lw_mm){
          this.errorMsg = "introduza valor";
        return false;
        }
        
      this.errorMsg = "";
      return true
    },
    isFormValid(){

        if(!this.isCodigoValid){
            return false
        }

        if(!this.isNomeValid){
            return false
        }

        if(!this.isWeff_pValid){
            return false
        }
        if(!this.isWeff_nValid){
            return false
        } 
        if(!this.isArValid){
            return false
        }  
        if(!this.isSigmaCValid){
            return false
        }  
        if(!this.isH_mmValid){
            return false
        } 

        if(!this.isB_mmValid){
            return false
        } 
        if(!this.isC_mmValid){
            return false
        } 
        if(!this.isT_mmValid){
            return false
        } 

        if(!this.isA_mmValid){
            return false
        } 
        if(!this.isP_kg_mValid){
            return false
        } 
        if(!this.isYg_mmValid){
            return false
        } 
        if(!this.isZg_mmValid){
            return false
        } 
        if(!this.isLy_mmValid){
            return false
        } 

        if(!this.isWy_mmValid){
            return false
        } 
        if(!this.isLz_mmValid){
            return false
        } 
        if(!this.isWz_mmValid){
            return false
        } 
        if(!this.isYs_mmValid){
            return false
        } 
        if(!this.isZs_mmValid){
            return false
        } 
        if(!this.isLt_mmValid){
            return false
        } 
        if(!this.isLw_mmValid){
            return false
        } 

        return true
    }
  },
  methods:{
      reset() {
      this.errorMsg = false;
    },
    create() {
        this.$axios.$post("/api/variantes", {
            codigo:this.codigo,
            produtoNome: this.nomeProduto,
            nome: this.nome,
            weff_p: this.weff_p,
            weff_n: this.weff_n,
            ar: this.ar,
            sigmaC: this.sigmaC,
            h_mm:this.h_mm,
            b_mm: this.b_mm,
            c_mm: this.c_mm,
            t_mm: this.t_mm,
            a_mm: this.a_mm,
            p_kg_m: this.p_kg_m,
            yg_mm: this.yg_mm,
            zg_mm: this.zg_mm,
            ly_mm: this.ly_mm,
            wy_mm: this.wy_mm,
            lz_mm: this.lz_mm,
            wz_mm: this.wz_mm,
            ys_mm: this.ys_mm,
            zs_mm: this.zs_mm,
            lt_mm: this.lt_mm,
            lw_mm: this.lw_mm
        })
        .then(() => {
          this.$router.push("/produtos/" + this.nomeProduto)
        })
        .catch((error) => {
          this.errorMsg = error.response.data
          
        })
    }
  }
};
</script>