<template>
  <div class="container">
    <logout></logout>
    <div class="columns">
      <div class="column is-10 is-offset-1" id="tabla">
        <div class="column ">
        <form @submit.prevent="Agregartarea">
          <div class="field">
            <input class="input" type="text" placeholder="Escribe tu Tarea" v-model="task.nombre">
          </div>
          <div class="field">
            <textarea class="textarea" placeholder="Textarea" v-model="task.body"></textarea> 
          </div>
          <template v-if="edit==false">
          <button class="button is-success is-fullwidth">Enviar</button>
          </template>
          <template v-else>
            <button class="button is-success is-fullwidth">Actualizar</button>
          </template>
        </form>
        
        </div>

      </div>
      </div>
      <div class="columns is-mobile is-centered">
        <div class="column is-half">
        
          <div class="users">
            <button class="button is-primary is-fullwidth"><nuxt-link to="/home/me">Ver mis tareas</nuxt-link></button>
          <ul v-for="user in users">
            <li><nuxt-link :to="`/home/${user.id}`">{{user.name}}</nuxt-link></li>
          </ul>
          </div>
        </div>
      
    </div>
  </div>
</template>

<script>

import axios from 'axios';
import logout from '../../components/logout'

class task{
  constructor(user_id, nombre, body){
    this.user_id = user_id;
    this.nombre = nombre;
    this.body = body;
  }
}


export default {
  components: {
logout
},
  data(){
  return{
    task: new task(),
    tarea: [],
    edit: false,
    tareditar : '',
    users : [],
    me: []
  }
},
created () {
 this.Obtenerusuarios();
 this.Obtenertarea();
 this.yo();
},
methods:{ 
  
  Agregartarea(){
    if(this.edit==false){
    console.log(this.task)
    
    // try -> Exception
    
      this.$axios.post('users/newtasks',{
      user_id:this.me.id,
      nombre: this.task.nombre,
      body: this.task.body
    })
.then(function (response) {
 console.log(response);
}).then(data =>{
  this.Obtenertarea();
})
.catch(function (error) {
 console.log(error);
 alert('limites execidos')
});
this.task.nombre = "",
this.task.body = ""
}
},

//le cambie el response.data.data por que dejó de funcionar cuando puse el metodo put

Obtenertarea(){
this.$axios.get('users/gettasks')
.then(response => {
  this.tareas = response.data;
  console.log(response.data);
})
.catch(e => {
  console.log(e)
    // Capturamos los errores
})
},

Obtenerusuarios(){
this.$axios.get('getusers')
.then(response => {
  this.users = response.data;
  console.log(response.data);
})
.catch(e => {
  console.log(e)
    // Capturamos los errores
})
},

yo(){
this.$axios.get('me')
.then(response => {
  this.me = response.data.data.data;
  console.log(response.data.data.data);
})
.catch(e => {
  console.log(e)
    // Capturamos los errores
})
},

actualizar(id){
  fetch('tasks/'+id)
  .then(res => res.json())
  .then(data => {this.task = new task(data.nombre,data.body);
  this.tareditar=data.id
  this.edit=true;
  }),
  

console.log(id)
}
}
}


</script>

<style>
#tabla{
  background-color: gray;
  margin: 5em;
}
</style>