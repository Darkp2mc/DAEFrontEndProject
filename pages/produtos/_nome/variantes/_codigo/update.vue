<template>
  <b-container>
    <h3>Atualizar Variante  </h3>
    <b-form @submit.prevent="update" :disabled="!isFormValid">
      
      <p>Insira o nome da Variante</p>
      <b-input
        v-model="nome"
        :state="isNomeValid"
        required
      />
      <p>Insira o Weff_p da Variante</p>
      <b-input
        v-model="weff_p"
        :state="isWeff_pValid"
        required  
      />
      <p>Insira o Weff_n da Variante</p>
      <b-input
        v-model="weff_n"
        :state="isWeff_nValid"
        required
      />
      <p>Insira o Ar da Variante</p>
      <b-input
        v-model="ar"
        :state="isArValid"
        required
      />
      <p>Insira o sigmaC da Variante</p>
      <b-input
        v-model="sigmaC"
        :state="isSigmaCValid"
        required
      />
      <p>Insira o H_mm da Variante</p>
      <b-input
        v-model="h_mm"
        :state="isH_mmValid"
        required
      />
      <p>Insira o B_mm da Variante</p>
      <b-input
        v-model="b_mm"
        :state="isB_mmValid"
        required
      />
      <p>Insira o C_mm da Variante</p>
      <b-input
        v-model="c_mm"
        :state="isC_mmValid"
        required
      />
      <p>Insira o T_mm da Variante</p>
      <b-input
        v-model="t_mm"
        :state="isT_mmValid"
        required
      />
      <p>Insira o A_mm da Variante</p>
      <b-input
        v-model="a_mm"
        :state="isA_mmValid"
        required
      />
      <p>Insira o P_Kg_M da Variante</p>
      <b-input
        v-model="p_kg_m"
        :state="isP_kg_mValid"
        required
      />
      <p>Insira o Yg_mm da Variante</p>
      <b-input
        v-model="yg_mm"
        :state="isYg_mmValid"
        required
      />
      <p>Insira o Zg_mm da Variante</p>
      <b-input
        v-model="zg_mm"
        :state="isZg_mmValid"
        required
      />
      <p>Insira o Ly_mm da Variante</p>
      <b-input
        v-model="ly_mm"
        :state="isLy_mmValid"
        required
      />
      <p>Insira o Wy_mm da Variante</p>
      <b-input
        v-model="wy_mm"
        :state="isWy_mmValid"
        required
      />
      <p>Insira o Lz_mm da Variante</p>
      <b-input
        v-model="lz_mm"
        :state="isLz_mmValid"
        required
      />
      <p>Insira o Wz_mm da Variante</p>
      <b-input
        v-model="wz_mm"
        :state="isWz_mmValid"
        required
      />
      <p>Insira o Ys_mm da Variante</p>
      <b-input
        v-model="ys_mm"
        :state="isYs_mmValid"
        required
      />
      <p>Insira o Zs_mm da Variante</p>
      <b-input
        v-model="zs_mm"
        :state="isZs_mmValid"
        required
      />
      <p>Insira o Lt_mm da Variante</p>
      <b-input
        v-model="lt_mm"
        :state="isLt_mmValid"
        required
      />
      <p>Insira o Lw_mm da Variante</p>
      <b-input
        v-model="lw_mm"
        :state="isLw_mmValid"
        required
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <button type="reset" @click="reset">Reset</button>
      <button @click.prevent="update" :disabled="!isFormValid">Atualizar</button>
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
      fabricanteNome: this.$auth.user.sub,
      nomeProduto: this.nomeProduto,
      errorMsg: null,
      nome: this.nome,
      weff_p: this.weff_p,
      weff_n: this.weff_n,
      ar: this.ar,
      sigmaC: this.sigmaC,
      h_mm: this.h_mm,
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
      lw_mm: this.lw_mm,
    };
  },
  created(){
      this.$axios.$get(`/api/variantes/${this.codigo}`)
          .then((variante)=> (this.nome = variante.nome,
            this.nomeProduto= variante.produtoNome,
            this.weff_p= variante.weff_p,
            this.weff_n = variante.weff_n,
            this.ar = variante.ar,
            this.sigmaC = variante.sigmaC,
            this.h_mm = variante.h_mm,
            this.b_mm = variante.b_mm,
            this.c_mm = variante.c_mm,
            this.t_mm = variante.t_mm,
            this.a_mm = variante.a_mm,
            this.p_kg_m = variante.p_kg_m,
            this.yg_mm = variante.yg_mm,
            this.zg_mm = variante.zg_mm,
            this.ly_mm = variante.ly_mm,
            this.wy_mm = variante.wy_mm,
            this.lz_mm = variante.lz_mm,
            this.wz_mm = variante.wz_mm,
            this.ys_mm = variante.ys_mm,
            this.zs_mm = variante.zg_mm,
            this.lt_mm = variante.lt_mm,
            this.lw_mm = variante.lw_mm
            ))
  },
  computed: {
    codigo(){
      return this.$route.params.codigo
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
    update() {
        this.$axios.$put(`/api/fabricantes/${this.fabricanteNome}/produtos/${this.nomeProduto}/variantes/${this.codigo}`, {
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