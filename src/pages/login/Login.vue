<template>
  <LoginTemplate>
    <span slot="menuEsquerdo">
      <img src="https://i.ytimg.com/vi/nvJQZyBBUMw/maxresdefault.jpg" alt="O dinheiro vem pra confundir o amor" style="width:100%">
    </span>
    <span slot="principal">
      <h2>Login</h2>
      <input type="email" placeholder="Email:" v-model="usuario.email">
      <input type="password" placeholder="Senha:" v-model="usuario.password">
      <button class="btn red" v-on:click="logar()">Entrar</button>
      <router-link class="btn grey" to="/cadastro">Ainda não possuo uma conta</router-link>
    </span>
  </LoginTemplate>
</template>

<script>
import LoginTemplate from '@/templates/LoginTemplate'
import axios from 'axios'

export default {
  name: 'Login',
  data () {
    return {
      usuario: {
        email: '',
        password: ''
      }
    }
  },
  components: {
    LoginTemplate
  },
  methods: {
    logar () {
      axios.post('http://localhost:8000/api/login', {
        email: this.usuario.email,
        password: this.usuario.password
      }).then(response => {
        if (response.data.token) {
          sessionStorage.setItem('usuario', JSON.stringify(response.data))
          this.$router.push('/')
        } else if (response.data.status === false) {
          alert('Login não existe')
        } else {
          let erros = ''
          for (let erro of Object.values(response.data)) { // transforma response.data em um array de valores
            erros += erro + ' '
          }
          alert(erros)
        }
      }).catch(e => {
        console.log(e)
        alert(e + ' Tente novamente mais tarde ')
      })
    }
  }
}
</script>

<style scoped>

</style>
