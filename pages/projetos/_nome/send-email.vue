<template>
  <div>
    <h1>Send an E-mail to  {{projeto.clienteUsername}}</h1>
    <form @submit.prevent="send">
      <div>
        subject:
        <input v-model="subject" type="text" />
      </div>
      <div>
        message:
        <textarea v-model="message" name="message"></textarea>
      </div>
      
      <nuxt-link class="btn btn-primary" :to="`/projetos/${this.nome}`">Voltar Atras
      </nuxt-link>
      &nbsp;
      <button class="btn btn-success" @click.prevent="send">Enviar</button>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      projeto: {},
      subject: null,
      message: null,
    };
  },
  created() {
    this.$axios.$get(`/api/projetos/${this.nome}`).then((projeto) => {
      this.projeto = projeto;
    });
  },
  computed: {
    nome() {
      return this.$route.params.nome;
    },
  },
  methods: {
    send() {
      this.$axios
        .$post(`/api/projetos/${this.nome}/email/send`, {
          subject: this.subject,
          message: this.message,
        })
        .then((msg) => {
          this.$toast.success(msg).goAway(1500)
          this.$router.push(`/projetos/${this.nome}`)
        })
        .catch((error) => {
          this.$toast.error("error sending the e-mail").goAway(3000);
        });
    },
  },
};
</script>