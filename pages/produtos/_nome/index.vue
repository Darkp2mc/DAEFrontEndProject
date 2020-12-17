<template>
    <b-container>
        <h3>Detalhes do produto  {{produto.nome}} </h3>
        <p>Tipo: {{produto.tipo}}</p>
        <p>Familia: {{produto.familia}}</p>
        <p>Fabricante: {{produto.fabricanteUsername}}</p>
        <h4>Dimensoes</h4>
        <p>E: {{produto.e}}</p>
        <p>N: {{produto.n}}</p>
        <p>G: {{produto.g}}</p>
        <h4>Variantes</h4>
        <b-table v-if="variantes.length" striped over :items="variantes" :fields="variantesFields">
        </b-table>
        <p v-else> Sem Variantes</p>
       
    </b-container>
</template>

<script>
export default {
    data() {
        return {
            produto: {},
            variantesFields: [
                'codigo',
                'nome',
                'weff_p',
                'weff_n',
                'ar',
                'sigmaC',
                'h_mm',
                'b_mm',
                'c_mm',
                't_mm',
                'a_mm',
                'p_kg_m',
                'yg_mm',
                'zg_mm',
                'ly_mm',
                'wy_mm',
                'lz_mm',
                'wz_mm',
                'ys_mm',
                'zc_mm',
                'lt_mm',
                'lw_mm'


                
            ],

        }
    },
    computed: {
        nome() {
            return this.$route.params.nome
        },
        variantes() {
            return this.produto.varianteDTOs || []
        }
        
    },
    created() {
        this.$axios.$get(`/api/produtos/${this.nome}`)
            .then((produto)=> (this.produto = produto || {}))

    }
    
}
</script>