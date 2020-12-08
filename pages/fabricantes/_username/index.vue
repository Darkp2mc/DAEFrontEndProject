<template>
    <b-container>
        <h4>Detalhes do Fabricante</h4>
        <p>Username: {{fabricante.username}}</p>
        <p>Username: {{fabricante.nome}}</p>
        <p>Username: {{fabricante.email}}</p>
        <h4>Produtos</h4>
        <b-table v-if="produtos.length" striped over :items="produtos" :fields="produtoFields">
        </b-table>
        <p v-else>Sem Produtos</p>
    </b-container>
</template>

<script>
export default {
    data() {
        return {
            fabricante: {},
            produtoFields: [
                'nome',
                'familia',
                'tipo'
            ],
        }
    },
    computed: {
        username() {
            return this.$route.params.username;
        },
        produtos(){
            return this.fabricante.produtoDTOS || []
        }
    }, 
    created(){
        this.$axios
            .$get(`/api/fabricantes/${this.username}`)
                .then((fabricante) => (this.fabricante = fabricante || {}))
  
    }
}
</script>