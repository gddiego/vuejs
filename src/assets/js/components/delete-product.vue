<template>
    <div id="delete-product">
        <h1>Deletar produto {{ product.name }}</h1>

        <p><router-link :to="{ name: 'all_products' }">voltar para listagem</router-link></p>

        <notification v-bind:notifications="notifications"></notification>

        <form v-on:submit.prevent="deleteProduct">
            <p><button class="btn btn-danger">Deletar Produto</button></p>
        </form>
    </div>
</template>

<script>
    import Notification from './notifications.vue';

    export default{
        data(){
            return{
                product:{},
                notifications:[]
            }
        },

        created: function(){
            this.getProduct();
        },

        methods: {
            getProduct: function()
            {
                this.$http.get('http://localhost:3000/api/product/' + this.$route.params.id).then((response) => {
                    this.product = response.body;
                }, (response) => {

                });
            },

            deleteProduct: function()
            {
                this.$http.delete('http://localhost:3000/api/product/delete/' + this.$route.params.id, this.product, {
                    headers : {
                        'Content-Type' : 'application/json'
                    }
                }).then((response) => {
                    this.$router.push({name: 'all_products'})
                }, (response) => {
                    this.notifications.push({
                        type: 'danger',
                        message: 'Produto nao foi deletado'
                    });
                });
            }
        },

        components: {
            'notification' : Notification
        }
    }
</script>
