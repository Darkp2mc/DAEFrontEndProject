<template>
    <b-container>
        <h4>Detalhes da Estrutura {{estrutura.nome}}</h4>
        <p>Nome do Projeto: {{estrutura.projetoNome}}</p>
        <p>Tipo de Produto: {{estrutura.tipoDeProduto}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Lage'|| estrutura.tipoDeProduto=='Painel'">Numero de Vaos: {{estrutura.numeroDeVaos}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Lage'|| estrutura.tipoDeProduto=='Painel'">Comprimento do Vao: {{estrutura.comprimentoDaVao}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Painel'">Aplicação: {{estrutura.aplicacao}}</p>
        <p v-if="estrutura.tipoDeProduto=='Lage'">Altura da Lage: {{estrutura.alturaDaLage}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Lage'|| estrutura.tipoDeProduto=='Painel'">Sobrecarga: {{estrutura.sobrecarga}}</p>
    
        <h4>Variantes</h4>
        <b-table v-if="variantes.length" striped over :items="variantes" :fields="variantesFields">
        </b-table>
        <p v-else> Sem Variantes</p>

        <b-button variant="primary" :to="`/projetos/${estrutura.projetoNome}`">Voltar</b-button>
        <b-button variant="success" v-if="this.$auth.user.groups.includes('Projetista')" :to="`${nome}/variantes/create`">Adicionar Variante</b-button>
       <b-button variant="success" v-if="this.$auth.user.groups.includes('Cliente')" >Aprovar</b-button>
       <b-button variant="danger" v-if="this.$auth.user.groups.includes('Cliente')" >Rejeitar</b-button>
    </b-container>
</template>

<script>
export default {
    data() {
        return {
            estrutura: {},
            variantesFields: [
                'codigo',
                'produtoNome',
                'nome'
            ],
            rejeitada: null

        }  
    },
    computed:{
        nome() {
            return this.$route.params.nome
        },
        variantes() {
            return this.estrutura.varianteDTOs || []
        }
    },
    created() {
        this.$axios.$get(`/api/estruturas/${this.nome}`)
            .then((estrutura)=> (this.estrutura = estrutura || {}))
    },
   

    
}
</script>