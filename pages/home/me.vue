<template>
    <div class="container">
        <div class="columns is-mobile is-centered">
            <div class="column is-half">
                <h1 class="title is-1">Todas mis tareas</h1>
            </div>
        </div>
        <div class="columns is-mobile is-centered">
            <div class="column is-half">
               <nuxt-link to="/home"><p>Regresar</p></nuxt-link>
                <table class="table">
                    <thead>
                        <tr>
                            <th>
                                <P>Todas las tareas</P>
                            </th>
                            <th>
                                <p>Eliminar</p>
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="tarea in tareas">
                             <td><nuxt-link :to="`/home/task/${tarea.id}`">{{tarea.nombre}}</nuxt-link></td>
                            <td><button @click="eliminartarea(tarea.id)" class="button is-danger">X</button></td>
                        </tr>  
                    </tbody>
                    
                </table>
                
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        middleware: ['auth'],
        data(){
            return{
                me: {},
                id: 0,
                tareas: []
            }
        },
        created(){
            this.mistareas();
        },
        mistareas() {
        },
        methods : {
            mistareas(){
                this.$axios.get('me')
                    .then(response => {
                        this.me = response.data.data.data;
                        this.id = response.data.data.data.id
                        
                        this.$axios.get('users/gettask/'+this.id)
                        .then(response => {
                            this.tareas = response.data;
                        })
                    })     
                
                },
            eliminartarea(id){
                this.$axios.delete('users/deletetask/'+id)
                .then(data =>{
                this.mistareas();
                })
                }
    }
}

    
</script>

<style lang="scss" scoped>

</style>