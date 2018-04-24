<template>
  <LoginTemplate>
    <span slot="menuEsquerdo">
      <img src="https://i.ytimg.com/vi/nvJQZyBBUMw/maxresdefault.jpg" alt="O dinheiro vem pra confundir o amor" style="width:100%">
    </span>
    <span slot="principal">
        <h2>Cadastrar</h2>
        <input type="text" v-model="usuario.name" placeholder="Nome:">
        <input type="email" v-model="usuario.email" placeholder="Email:">
        <input type="password" v-model="usuario.password" placeholder="Senha:">
        <input type="password" v-model="usuario.password_confirmation" placeholder="Confirme sua Senha:">
        <button class="btn red" v-on:click="cadastro()">Cadastrar</button>
        <router-link class="btn grey" to="/login">Já possuo uma conta</router-link>

        <ul v-if="errors && errors.length">
          <li v-for="error of errors">
            {{error.message}}
          </li>
        </ul>
    </span>
  </LoginTemplate>
</template>

<script>
import LoginTemplate from '@/templates/LoginTemplate'
import axios from 'axios'

export default {
  name: 'Cadastro',
  data () {
    return {
      usuario: {
        email: '',
        password: '',
        password_confirmation: '',
        name: ''
      },
      errors: []
    }
  },
  components: {
    LoginTemplate
  },
  methods: {
    cadastro () {
      console.log(this.usuario)
      axios.post('http://localhost:8000/api/cadastro', {
        email: this.usuario.email,
        password: this.usuario.password,
        password_confirmation: this.usuario.password_confirmation,
        name: this.usuario.name
      }).then(response => {
        console.log(response)
        if (response.data.token) {
          sessionStorage.setItem('usuario', JSON.stringify(response.data))
          this.$router.push('/')
        } else if (response.data.status === false) {
          alert('Erro no cadastro')
        }
      }).catch(e => {
        console.log(e) // this.errors.push(e)
      })
    }
  }
}
</script>

<style scoped>

</style>
