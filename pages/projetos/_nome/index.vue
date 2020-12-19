<template>
    <b-container>
        <h4>Detalhes do Projeto</h4>
        <p>Nome: {{projeto.nome}}</p>
        <p v-if="this.$auth.user.groups.includes('Cliente')">Projetista: {{projeto.projetistaUsername}}</p>
        <p v-if="this.$auth.user.groups.includes('Projetista')">Cliente: {{projeto.clienteUsername}}</p>
        <p>Comentarios: {{projeto.comentario}}</p>
        <p v-if="projeto.estado==0">Em Desenvolvimento</p>
        <p v-if="projeto.estado==2">Terminado</p>
        <p v-if="projeto.estado==1">Aceite pelo Cliente</p>
        <p v-if="projeto.estado==-1">Recusado pelo Cliente</p>

        <h4>Estruturas</h4>
        <b-table v-if="estruturas.length && this.$auth.user.groups.includes('Cliente')" striped over :items="estruturas" :fields="estruturasFields">
            <template v-slot:cell(actions)="row">
                <nuxt-link class="btn btn-primary" :to="`/projetos/${projeto.nome}/estruturas/${row.item.nome}`" >Detalhes</nuxt-link>
            </template>
        </b-table>
        <b-table v-if="estruturas.length && this.$auth.user.groups.includes('Projetista')" striped over :items="estruturas" :fields="estruturasFields">
            <template v-slot:cell(actions)="row">
                <nuxt-link class="btn btn-primary" :to="`/projetos/${projeto.nome}/estruturas/${row.item.nome}`" >Detalhes</nuxt-link>
                <b-button variant="danger" v-on:click="deleteEstrutura(row.item.nome)">Eliminar</b-button>
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
        <b-button class="btn btn-info" v-if="this.$auth.user.groups.includes('Projetista') && projeto.estado!=2" v-on:click="terminar()">Terminar Projeto</b-button>
        <b-button variant="warning" v-if="this.$auth.user.groups.includes('Cliente')" :to="`/clientes/${projeto.clienteUsername}`">Voltar</b-button>
        <nuxt-link class="btn btn-primary" v-if="this.$auth.user.groups.includes('Cliente')" :to="`/projetos/${projeto.nome}/upload`">Enviar Ficheiro</nuxt-link>
        <b-button variant="success" v-if="this.$auth.user.groups.includes('Cliente')  && projeto.estado!=2 && projeto.estado != 1" v-on:click="showFormsAcept()">Aceitar Projeto</b-button>
        <b-button variant="danger" v-if="this.$auth.user.groups.includes('Cliente')  && projeto.estado!=2 && projeto.estado != -1" v-on:click="showFormsDecline()">Rejeitar Projeto</b-button>
            <br>
        <form v-if="showFormAcept" @submit.prevent="aceitarProjeto">    
            <p>Comentario: </p>
            <input v-model="comentario" type="text"/>
            <br>
            <button class="btn btn-primary" v-on:click="fechar()" >Cancelar</button>
            <button class="btn btn-success"  @click.prevent="aceitarProjeto" >Send</button>
        </form>
        <form v-if="showFormDecline" @submit.prevent="rejeitarProjeto">
            <p>Comentario: </p>
            <input v-model="comentario" type="text"/>
            <br>
            <button class="btn btn-primary" v-on:click="fechar()" >Cancelar</button>
            <button class="btn btn-danger" @click.prevent="rejeitarProjeto" >Send</button>
        </form>
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
          'estado',
          'actions'
      ],
      
      documentsFields: ['filename', 'actions'],
      showFormAcept: false, 
      showFormDecline: false,
      comentario: ''
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
      terminar(){
          this.$axios.$put(`api/projetos/${this.nome}/terminar`, {})
          .then( () => {
                  window.location.reload()
                   
                })
                .catch(errors=>
                    console.log(errors)
                )
      }, 
      showFormsAcept(){
        this.showFormAcept = true
      },
      showFormsDecline(){
          this.showFormDecline = true
      },
      fechar(){
          this.showFormAcept = false
          this.showFormDecline = false
          this.comentario = ''
      },
      aceitarProjeto() {
          this.$axios.$put(`api/projetos/${this.nome}/aceitar`, {
              nome: this.nome,
              comentario: this.comentario
          })
            .then( (msg) => {
                this.$toast.success(msg).goAway(1500)
                window.location.reload()
                this.comentario = ''
                this.showFormAcept = false
            })
            .catch(errors=>
                this.$toast.error("error making comment").goAway(3000)
            )
      },
      rejeitarProjeto(){
          this.$axios.$put(`api/projetos/${this.nome}/rejeitar`, {
              nome: this.nome,
              comentario: this.comentario
          })
            .then( (msg) => {
                this.$toast.success(msg).goAway(1500)
               window.location.reload() 
               this.comentario = ''
                this.showFormDecline = false
            })
            .catch(errors=>
                this.$toast.error("error making comment").goAway(3000)
            )
      },
      deleteEstrutura(nomeEstrutura){
          this.$axios.$put(`api/projetos/${this.nome}/estrutura/${nomeEstrutura}/remover`)
            .then(()=>{
                window.location.reload()
            })
            .catch(errors=>
            console.log(errors)
            )

      }

  }
}
</script>
