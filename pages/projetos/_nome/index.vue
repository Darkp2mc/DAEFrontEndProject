<template>
    <b-container>
        <h4>Detalhes do Projeto</h4>
        <p>Nome: {{projeto.nome}}</p>
        <p v-if="this.$auth.user.groups.includes('Cliente')">Projetista: {{projeto.projetistaUsername}}</p>
        <p v-if="this.$auth.user.groups.includes('Projetista')">Cliente: {{projeto.clienteUsername}}</p>
        <p>Comentarios: {{projeto.comentario}}</p>

        <h4>Estruturas</h4>
        <b-table v-if="estruturas.length" striped over :items="estruturas" :fields="estruturasFields">
            <template v-slot:cell(actions)="row">
                <nuxt-link class="btn btn-primary" :to="`/projetos/${projeto.nome}/estruturas/${row.item.nome}`" >Detalhes</nuxt-link>
                
            </template>
        </b-table>
        <p v-else> Sem Estruturas</p>

        

        <h4>Documentos</h4>
        <b-table v-if="documents.length" striped over :items="documents" :fields="documentsFields">
        <template v-slot:cell(actions)="row">
            <b-btn class="btn btn-link" @click.prevent="download(row.item)"
            target="_blank">Download</b-btn>
        </template>
        </b-table>
        <p v-else>Sem Documentos</p>

        <b-button variant="warning" v-if="this.$auth.user.groups.includes('Projetista')" :to="`/projetistas/${projeto.projetistaUsername}`">Voltar</b-button>
        <nuxt-link class="btn btn-success" v-if="this.$auth.user.groups.includes('Projetista')" :to="`/projetos/${projeto.nome}/send-email`">Enviar e-mail</nuxt-link>
        <nuxt-link class="btn btn-primary" v-if="this.$auth.user.groups.includes('Projetista')" :to="`/projetos/${projeto.nome}/update`">Atualizar Projeto</nuxt-link>
        <nuxt-link class="btn btn-danger" v-if="this.$auth.user.groups.includes('Projetista')" :to="`/projetos/${projeto.nome}/estruturas/create`">Criar Estrutura </nuxt-link>
        <b-button variant="warning" v-if="this.$auth.user.groups.includes('Cliente')" :to="`/clientes/${projeto.clienteUsername}`">Voltar</b-button>
        <nuxt-link class="btn btn-primary" v-if="this.$auth.user.groups.includes('Cliente')" :to="`/projetos/${projeto.nome}/upload`">Enviar Ficheiro</nuxt-link>
        <nuxt-link class="btn btn-success"  v-if="this.$auth.user.groups.includes('Cliente')" :to="`/projetos/${projeto.nome}/comentario`">Fazer Comentario</nuxt-link>
         
    </b-container>
</template>

<script>
export default {
    data() {
    return {
      projeto: {},
      estruturasFields : [
          'nome',
          'tipoDeProduto',
          'rejeitada',
          'actions'
      ],
      
      documentsFields: ['filename', 'actions']
    }
  },

  computed: {
      nome() {
          return this.$route.params.nome
         
      },
      estruturas() {
          return this.projeto.estruturas || []
      },
      documents() {
          return this.projeto.documentos || []
      }
  },

  created() {
        if(this.$auth.user.groups.includes("Projetista")){
           this.$axios
                .$get(`/api/projetistas/${this.$auth.user.sub}/projetos/${this.nome}`)
                    .then((projeto)=> (this.projeto = projeto || {}))
        }else{
             this.$axios.$get(`/api/clientes/${this.$auth.user.sub}/projetos/${this.nome}`)
            .then((projeto)=> (this.projeto = projeto || {}))
        }
            
  },

  methods: {
      download(fileToDownload) {
        const documentId = fileToDownload.id

        this.$axios.$get('/api/documents/download/' + documentId , {responseType:'arraybuffer'})
            .then(file =>{
                const url = window.URL. createObjectURL (new Blob([file]))
                const link = document.createElement('a')
                link.href = url
                link.setAttribute('download', fileToDownload.filename)
                document.body.appendChild(link)
                link.click()
                })
      },

  }
}
</script>
