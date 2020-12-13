<template>
    <b-container>
        <h4>Criar novo Produto</h4>
        <form @submit.prevent="create" :disabled="!isFormValid">
            <b-input
                v-model.trim="nome"
                :state="isNomeValid"
                required
                placeholder="Insira o nome do produto"
            />
            <b-select 
                v-model.trim="tipo"
                :state = "isTipoValid"
                :options="tipoOptions"
                required
            />
        </form>

        <nuxt-link class="btn btn-primary" :to="`/fabricantes/${fabricanteNome}`">Return</nuxt-link>
    </b-container>
</template>
<script>
export default {
    data(){
        return {
            fabricanteNome: this.$auth.user.sub,
            nome: null
        }
    }
    
}
</script>