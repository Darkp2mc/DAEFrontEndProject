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
            <template v-slot:cell(actions)= "row">
                <nuxt-link class="btn btn-success" :to="`/variantes/${row.item.nome}/update`">Atualizar</nuxt-link>
                <b-button variant="danger" v-on:click="deleteVariante(row.item.codigo)">Eliminar</b-button>
            </template>
        </b-table>
        <p v-else> Sem Variantes</p>

        <nuxt-link class="btn btn-primary" :to="`/fabricantes/${produto.fabricanteUsername}`" >Voltar Atras</nuxt-link>
        <nuxt-link class="btn btn-success" :to="`/produtos/${produto.nome}/variantes/create`"> Criar Variante</nuxt-link>
       
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
                'lw_mm',
                'actions'                
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

    },
    methods: {
        deleteVariante(codigoVariante){

        }
    }
    
}
</script>