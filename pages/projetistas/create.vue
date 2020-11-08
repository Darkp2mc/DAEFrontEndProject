<template>
  <div>
    <h1>Create a new Projetista</h1>
    <form @submit.prevent="create" :disabled="!isFormValid">
      <b-input
        v-model.trim="username"
        :state="isUsernameValid"
        required
        placeholder="Enter your username"
      />
      <b-input
        v-model="password"
        :state="isPasswordValid"
        required
        placeholder="Enter your password"
      />
      <b-input
        v-model.trim="nome"
        :state="isNameValid"
        required
        placeholder="Enter your name"
      />
      <b-input
        
        v-model.trim="email"
        type="email"
        :state="isEmailValid"
        required
        placeholder="Enter your e-mail"
      />
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/projetistas">Return</nuxt-link>
      <button type="reset" @click="reset">RESET</button>
      <button @click.prevent="create" :disabled="!isFormValid">CREATE</button>
    </form>
  </div>
</template>
<script>
export default {
    data() {
    return {
      username: null,
      password: null,
      nome: null,
      email: null,
      errorMsg: false,
    };
  },
  computed: {
    isUsernameValid() {
      if (!this.username) {
        return null;
      }
      let usernameLen = this.username.length;
      if (usernameLen < 3 || usernameLen > 15) {
        return false;
      }
      return true;
    },
    isPasswordValid() {
      if (!this.password) {
        return null;
      }
      let passwordLen = this.password.length;
      if (passwordLen < 3 || passwordLen > 255) {
        return false;
      }
      return true;
    },
    isNameValid() {
      if (!this.nome) {
        return null;
      }
      let nomeLen = this.nome.length;
      if (nomeLen < 3 || nomeLen > 25) {
        return false;
      }
      return true;
    },
    isEmailValid() {
      if (!this.email) {
        return null;
      }

        return true;
    },
   
    isFormValid() {
      if (!this.isUsernameValid) {
        return false;
      }
      if (!this.isPasswordValid) {
        return false;
      }
      if (!this.isNameValid) {
        return false;
      }
      if (!this.isEmailValid) {
        return false;
      }
      
      return true;
    },
  },
  methods: {
    reset() {
      this.errorMsg = false;
    },
    create() {
      this.$axios
        .$post("/api/projetistas", {
          username: this.username,
          password: this.password,
          nome: this.nome,
          email: this.email,
          courseCode: this.courseCode,
        })
        .then(() => {
          this.$router.push("/projetistas");
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
}
</script>
