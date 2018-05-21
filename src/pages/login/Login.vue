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

export default {
  name: 'Login',
  data () {
    return {
      usuario: {
        email: '',
        password: ''
      },
      erros: ''
    }
  },
  components: {
    LoginTemplate
  },
  methods: {
    logar () {
      this.$http.post(this.$url + '/api/login', {
        email: this.usuario.email,
        password: this.usuario.password
      }).then(response => {
        if (response.data.status) {
          this.$store.commit('setUsuario', response.data.usuario)
          sessionStorage.setItem('usuario', JSON.stringify(response.data.usuario))
          this.$router.push('/')
        } else if (response.data.status === false && response.data.validacao) {
          // alert('Login não existe')
          // let erros = ''
          for (let erro of Object.values(response.data.erros)) { // transforma response.data em um array de valores
            this.erros += erro + ' '
          }
          alert('Erro no cadastro' + this.erros)
        } else {
          alert(this.erros)
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
