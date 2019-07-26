<template>
    <div class="container">
        <div class="columns is-mobile is-centered">
            <div class="column is-half">
                <h1 class="title is-1"> Tareas de {{this.user.name}}</h1>
            </div>
        </div>
        <div class="columns is-centered">
            <div class="column is-half">
                <nuxt-link to="/home">Regresar</nuxt-link>
                <ul v-for="task in tasks">
                    <li><nuxt-link :to="`/home/task/${task.id}`">{{task.nombre}}</nuxt-link></li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                user: {},
                tasks:[],
            }
        },
        created(){
           this.getone(),
           this.getbyid()
        },
        methods: {
            getone (){
                this.$axios.get(this.$route.params.id)
                .then(userresponse => {
                    this.user = userresponse.data;
                    
                    console.log(this.user);
                })
            },
        
        async getbyid (){
            this.$axios.get('users/gettask/'+this.$route.params.id)
                .then(taskresponse => {
                    this.tasks = taskresponse.data;
                    
                    console.log(this.task);
                })
        }
        }
    }
</script>

<style lang="scss" scoped>

</style>