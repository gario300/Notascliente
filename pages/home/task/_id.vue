<template>
    <div class="container">
        <div class="columns is-mobile is-centered">
            <div class="column is-half">
                <nuxt-link :to="`/home/${task.user_id}`">regresar</nuxt-link>
                <table class="table">
                    <thead>
                        <tr>
                            <th>Tarea</th>
                            <th>Descripcion</th>
                            <th v-if="currentuser == true">Accion</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                        <template v-if="edit == true">   
                            <td>
                                <form @submit.prevent="metodoput(task.id)">
                                    <input type="text" v-model="nombre" placeholder="nombre">
                                    <input type="text" v-model="body" placeholder="tarea">
                                    <button class="button is-success">Editar!</button>  
                                </form >   
                            </td>
                        </template > 
                        <template v-else> 
                            <td>{{task.nombre}}</td>
                            <td>{{task.body}}</td>
                            <td v-if="currentuser == true" @click="editar"><button class="button is-success">Edit</button></td>
                        </template > 
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <div class="columns is-mobile is-centered">
            <table class="table is-fullwidth">
                <thead>
                    <tr>
                        <th>Comentarios</th>
                    </tr>
                    <tr>
                        <th>Usuario</th>
                        <th>Comentario</th>
                    </tr>
                    </thead>
                    <tbody id="comentbox" >
                    <tr v-for="comentario in comentarios">
                        <td>{{comentario.username}}</td>
                        <td>{{comentario.coment}}</td>
                    </tr>
                </tbody>
            </table>
            
        </div>
        <div class="columns is-mobile is-centered">
            <div class="column is-half">
                    <form @submit.prevent="newcoment" class="form">
                        <div class="field">
                            <div class="control">
                                <textarea class="textarea is-large" v-model="coment" placeholder="Escribe un comentario"></textarea>
                            </div>
                        </div>
                        <button class="button is-success is-fullwidth">Comentar</button>
                    </form>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        data(){
            return{
                coment:'',
                task: {},
                currentuser: false,
                edit: false,
                me: [],
                id:0,
                nombre : '',
                body : '',
                comentarios: []
            }
        },
        created(){
            this.getonetask();
        },
        methods: {
          
          getonetask() {
              this.$axios.get('me')
                    .then(response => {
                        this.me = response.data.data.data;
                        this.id = response.data.data.data.id
                        
                    this.$axios.get('users/task/'+this.$route.params.id)
                    .then(taskresponse => {
                    this.task = taskresponse.data;
                    console.log(this.task)
                    
                    
                    if (this.me.id == this.task.user_id){
                        this.currentuser = true
                    }
                    this.$axios.get('users/coments/'+this.task.id)
                    .then( comentresponse => {
                    this.comentarios = comentresponse.data;
                    console.log(this.comentarios)
                })
                })
                })
          },

          editar(){
              this.edit = true
          },
          metodoput(id){
            this.$axios.put('users/taskedit/'+id,{
            user_id: this.id,
            nombre: this.nombre,
            body: this.body
            }).then(res => 
            this.getonetask(),
            this.nombre = "",
            this.body = "",
            this.edit=false)

          },
          newcoment(){
            this.$axios.post('users/newcoment',{
                task_id:this.task.id,
                username: this.me.name,
                coment: this.coment
                })
                .then(function (response) {
                console.log(response);
                }).then(data =>{
                this.coment = '',
                this.getcoments();
            })
          },
          getcoments(){
               this.$axios.get('users/coments/'+this.task.id)
                .then( response => {
                    this.comentarios = response.data;
                    console.log(this.comentarios)
                })

          }
          
         
        
    }
    }
</script>

<style scoped>
#comentbox {
    height: 20px;
    overflow-y: scroll;
}
</style>