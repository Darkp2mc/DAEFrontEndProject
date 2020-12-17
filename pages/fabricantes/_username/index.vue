<template>
    <b-container>
        <h4>Detalhes do Fabricante</h4>
        <p>Username: {{fabricante.username}}</p>
        <p>Nome: {{fabricante.nome}}</p>
        <p>Email: {{fabricante.email}}</p>
        <h4>Produtos</h4>
        <b-table v-if="produtos.length" striped over :items="produtos" :fields="produtoFields">
            <template v-slot:cell(actions)= "row">
                <nuxt-link class="btn btn-primary" :to="`/produtos/${row.item.nome}`">Details</nuxt-link>

                <b-button variant="danger" v-on:click="deleteProduto(row.item.nome)">DELETE</b-button>
            </template>
        </b-table>
        <p v-else>Sem Produtos</p>

        <nuxt-link class="btn btn-success"  :to="`/produtos/create`">Criar Produtos</nuxt-link>
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
                'tipo',
                'Actions'
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
    },
    methods: {
        deleteProduto(nomeProduto){
            this.$axios.
            $delete(`/api/fabricantes/${this.username}/produtos/${nomeProduto}`)
            .then(()=> {
                window.location.reload()
            })
            .catch(errors=>
                console.log(errors)
            )
        }
    }
}
</script>