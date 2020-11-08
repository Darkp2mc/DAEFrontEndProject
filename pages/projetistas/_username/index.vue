<template>
  <b-container>
    <h4>Projetista Details:</h4>
    <p>Username: {{ projetista.username }}</p>
    <p>Name: {{ projetista.nome }}</p>
    <p>Email: {{ projetista.email }}</p>
    <b-button variant="warning" to="/projetistas">Back</b-button>
  </b-container>
</template>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vue/1.0.18/vue.min.js"></script>
<script>
export default {
  data() {
    return {
      projetista: {}
    };
  },
  computed: {
    username() {
      return this.$route.params.username;
    }
  },
  created() {
    this.$axios
      .$get(`/api/projetistas/${this.username}`)
      .then((projetista) => (this.projetista = projetista || {}))
  }  
};
</script>
