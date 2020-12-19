<template>
    <div>
        <h1>Make a comment About {{nome}}</h1>

       
    </div>
</template>

<script>
export default {
    data(){
        return {
            projeto: {},
            username: null,
            comentario: null
        }
    },
    computed: {
        nome() {
          return this.$route.params.nome
      },
    },
    created(){
        this.$axios.$get(`/api/clientes/${this.$auth.user.sub}/projetos/${this.nome}`)
            .then((projeto)=> (this.projeto = projeto || {}))
    },
    methods:{
        send(){
            this.$axios
                .$post(`/api/clientes/${this.$auth.user.sub}/projetos/${this.nome}/comentario`, {
                    username: this.$auth.user.sub,
                    nome: this.nome,
                    comentario: this.comentario
                })
                .then((msg) => {
                this.$toast.success(msg).goAway(1500)
                this.$router.push("/projetos/"+ this.nome)
                })
                .catch((error) => {
                this.$toast.error("error making comment").goAway(3000);
                })
        }
    }
}
</script>