<template>
    <b-container>
        <b-table striped over :items="projetistas" :fields="fields">
            <template v-slot:cell(actions)="row">
                <nuxt-link class="btn btn-link" :to="`/projetistas/${row.item.username}`">
                Details
                </nuxt-link>
            </template>
        </b-table>
        <b-button variant="warning" to="/"> Back</b-button>
        
    </b-container>
</template>

<script>
export default {
    data(){
        return {
            fields: ['username', 'nome', 'email', 'actions'],
            projetistas: []
        }
    },
    created () {
        this.$axios.$get('api/projetistas').then((projetistas) => {this.projetistas = projetistas})
    }  
}
</script>

<style>
</style>