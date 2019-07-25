<template>
  <div class="container">
    <div class="columns is-centered">
      <div class="column is-half">
        
        
        
        <div v-if="mostrarf.valor == 1" class="form">
          <form @submit.prevent="login">
            <div class="field">
              <label class="label">email</label>
              <div class="control">
                <input class="input" v-model="email" type="email" placeholder="email">
              </div>
            </div>
            <div class="field">
              <label class="label">Password</label>
              <div class="control">
                <input class="input" v-model="password" type="password" placeholder="Password">
              </div>
            </div>
            <button class="button is-success is-fullwidth">Loggin!</button>
          </form>
          <button class="button is-success is-small" @click="mostrarform">Registate aquí</button>
        </div>
        
        <div v-else class="form">
            <form @submit.prevent="signup">
          <div class="field">
              <label class="label">Nombre</label>
              <div class="control">
                <input class="input" v-model="name" type="text" placeholder="Nombre">
              </div>
            </div>
            <div class="field">
              <label class="label">email</label>
              <div class="control">
                <input class="input" type="email" v-model="email" placeholder="email">
              </div>
            </div>
            <div class="field">
              <label class="label">Password</label>
              <div class="control">
                <input class="input" v-model="password" type="password" placeholder="Password">
              </div>
            </div>
            <button class="button is-success is-fullwidth">Registrame!</button>
            </form>

        </div>
      
      
      </div>
    </div>

  </div>
</template>

<script>
  export default {
   
  data() {
    return {
      name: '',
      email: '',
      password: '',
      error: null,
      mostrarf: {
        valor: 1
      }
    }
  },
  mounted () {
    setTimeout(() => {
      this.moveToDashboard()
    }, 1000)
  },

  methods: {
    moveToDashboard() {
      this.$router.push('/home')
    },
    
    mostrarform (){
      if (this.mostrarf.valor == 1){
        this.mostrarf.valor ++;
      }
    },

    async signup() {
      try {
        await this.$axios.post('signup', {
          name: this.name,
          email: this.email,
          password: this.password
        })

        await this.$auth.loginWith('local', {
          data: {
            email: this.email,
            password: this.password
          },
        })
        console.log('holamundo')
        this.$router.push('home')
        
      } catch (e) {
        console.log(e)
      }
    },
    async login() {
      try {
        await this.$auth.loginWith('local', {
          data: {
            email: this.email,
            password: this.password
          }
        })

        this.$router.push('home')
      } catch (e) {
        this.error = e.response.data.message
      }
    }
  }
  
  }
  



</script>

<style >

.form{
  position:relative;
  top: 100px;

}

.form button{
  margin-top: 1em;
}

</style>