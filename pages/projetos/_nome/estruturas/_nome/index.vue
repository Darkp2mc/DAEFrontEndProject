<template>
    <b-container>
        <h4>Detalhes da Estrutura {{estrutura.nome}}</h4>
        <p>Nome do Projeto: {{estrutura.projetoNome}}</p>
        <p>Tipo de Produto: {{estrutura.tipoDeProduto}}</p>
        <p v-if="estrutura.estado==1">Estado: Aceite</p>
        <p v-if="estrutura.estado==0">Estado: Por Decidir</p>
        <p v-if="estrutura.estado==-1">Estado: Rejeitada</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Lage'|| estrutura.tipoDeProduto=='Painel'">Numero de Vaos: {{estrutura.numeroDeVaos}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Lage'|| estrutura.tipoDeProduto=='Painel'">Comprimento do Vao: {{estrutura.comprimentoDaVao}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Painel'">Aplicação: {{estrutura.aplicacao}}</p>
        <p v-if="estrutura.tipoDeProduto=='Lage'">Altura da Lage: {{estrutura.alturaDaLage}}</p>
        <p v-if="estrutura.tipoDeProduto=='Perfil' || estrutura.tipoDeProduto=='Chapa' || estrutura.tipoDeProduto=='Lage'|| estrutura.tipoDeProduto=='Painel'">Sobrecarga: {{estrutura.sobrecarga}}</p>
    
        <h4>Variantes</h4>
        <b-table v-if="variantes.length" striped over :items="variantes" :fields="variantesFields">
            <template v-if="this.$auth.user.groups.includes('Projetista')" v-slot:cell(actions)="row">
                <b-button variant="primary"  @click.prevent="removerVariante(row.item)">Remover</b-button>
            </template>
        </b-table>
        <p v-else> Sem Variantes</p>

        <b-button variant="primary" :to="`/projetos/${estrutura.projetoNome}`">Voltar</b-button>
        <b-button variant="success" v-if="this.$auth.user.groups.includes('Projetista')" v-on:click="showVariantes()">Adicionar Variante</b-button>
        <b-button variant="success" v-if="this.$auth.user.groups.includes('Cliente')" v-on:click="aceitar()" >Aprovar</b-button>
        <b-button variant="danger" v-if="this.$auth.user.groups.includes('Cliente')" v-on:click="rejeitar()" >Rejeitar</b-button>

        <h4 v-if="showVariante">Variantes A Adicionar</h4>
        <b-table v-if="showVariante" striped over :items="variantesAAdicionar" :fields="variantesAAdicionarFields">
            <template v-slot:cell(actions)="row">
                <b-button variant="primary" @click.prevent="adicionarVariante(row.item)">Adicionar</b-button>
                
            </template>
        </b-table>
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
                'nome',
                'actions'
            ],
            rejeitada: null,
            showVariante: false,
            variantesAAdicionar: {},
            variantesAAdicionarFields: [
                'codigo',
                'produtoNome',
                'nome',
                'actions'
            ]

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
        
        this.$axios.$get(`/api/estruturas/${this.nome}/variantes/simulation`)
            .then((variantesAAdicionar)=> (this.variantesAAdicionar = variantesAAdicionar || {}))
    },
    methods : {
        rejeitar(){
            this.$axios.$put(`/api/estruturas/${this.nome}/rejeitar`)
                .then( () => {
                   this.$router.push("/projetos/"+this.estrutura.projetoNome);
                   
                })
                .catch(errors=>
                    console.log(errors)
                )
        },
        aceitar() {
           this.$axios.$put(`/api/estruturas/${this.nome}/aceitar`)
                .then( () => {
                   this.$router.push("/projetos/"+this.estrutura.projetoNome);
                   
                })
                .catch(errors=>
                    console.log(errors)
                )
        },
        showVariantes(){
            this.showVariante = true;
        },
        adicionarVariante(variante){
            const varianteCod = variante.codigo
            this.$axios.$put(`/api/estruturas/${this.nome}/variantes/`+varianteCod)
                .then(() => {
                 window.location.reload()
                   
                })
                .catch(errors=>
                    console.log(errors)
                )
        },
        removerVariante(variante){
            const varianteCod = variante.codigo
            this.$axios.$put(`/api/estruturas/${this.nome}/variantes/${varianteCod}/remove` )
                .then(() => {
                 window.location.reload()
                   
                })
                .catch(errors=>
                    console.log(errors)
                )

        }

    }
   

    
}
</script>