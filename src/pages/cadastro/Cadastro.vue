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
    </span>
  </LoginTemplate>
</template>

<script>
import LoginTemplate from '@/templates/LoginTemplate'

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
      // console.log(this.usuario)
      this.$http.post(this.$url + '/api/cadastro', {
        email: this.usuario.email,
        password: this.usuario.password,
        password_confirmation: this.usuario.password_confirmation,
        name: this.usuario.name
      }).then(response => {
        console.log(response)
        if (response.data.status) {
          this.$store.commit('setUsuario', response.data.usuario)
          sessionStorage.setItem('usuario', JSON.stringify(response.data.usuario))
          this.$router.push('/')
        } else if (response.data.status === false && response.data.validacao) {
          let erros = ''
          for (let erro of Object.values(response.data.erros)) { // transforma response.data em um array de valores
            erros += erro + ' '
          }
          alert('Erro no cadastro' + erros)
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
