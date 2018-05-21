<template>
  <div>
    <header>
      <NavBar url="/" cor="red darken-3" logo="Social">
        <li v-if="!usuario"><router-link to="/login">Entrar</router-link></li>
        <li v-if="!usuario"><router-link to="/cadastro">Cadastrar</router-link></li>
        <li v-if="usuario"><router-link to="/perfil">{{ usuario.name }}</router-link></li>
        <li v-if="usuario"><a v-on:click="sair()">Sair</a></li>
      </NavBar>
    </header>

    <main>
      <div class="container">
        <div class="row">
          <GridVue tamanho="6">
            <CardMenuVue>
              <slot name="menuEsquerdo" />
            </CardMenuVue>
          </GridVue>
          <GridVue tamanho="6">
            <slot name="principal" />
          </GridVue>
        </div>
      </div>
    </main>

    <FooterVue cor="red darken-3" logo="Social" descricao="You can use rows and columns here to organize your footer content." ano="2017">
        <li><a class="grey-text text-lighten-3" href="#!">Link 1</a></li>
        <li><a class="grey-text text-lighten-3" href="#!">Link 2</a></li>
        <li><a class="grey-text text-lighten-3" href="#!">Link 3</a></li>
        <li><a class="grey-text text-lighten-3" href="#!">Link 4</a></li>
    </FooterVue>
  </div>
</template>

<script>
import NavBar from '@/components/layout/NavBar'
import FooterVue from '@/components/layout/FooterVue'
import GridVue from '@/components/layout/GridVue'
import CardMenuVue from '@/components/layout/CardMenuVue'

export default {
  name: 'LoginTemplate',
  data () {
    return {
      usuario: false
    }
  },
  components: {
    NavBar,
    FooterVue,
    GridVue,
    CardMenuVue
  },
  created () {
    let usuarioAux = this.$store.getters.getUsuario
    if (usuarioAux) {
      this.usuario = this.$store.getters.getUsuario
      this.$router.push('/')
    }
  }
}
</script>

<style>
</style>
