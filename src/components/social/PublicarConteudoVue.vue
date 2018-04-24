<template>
  <div class="row">
    <GridVue class="input-field" tamanho="12">
      <textarea v-model="conteudo" class="materialize-textarea"></textarea>
      <label>O que está acontecendo?</label>
    </GridVue>
    <p>
      <GridVue v-if="conteudo" class="btn waves-effect waves-light" tamanho="2 offset-s10"><a v-on:click="cadastrarConteudo()">Publicar</a></GridVue>
    </p>
    <ErrosLista :errors="errors"/>
  </div>
</template>

<script>
import GridVue from '@/components/layout/GridVue'
import ErrosLista from '@/components/layout/ErrosLista'
import axios from 'axios'

export default {
  name: 'PublicarConteudoVue',
  data () {
    return {
      conteudo: '',
      usuario: {
        email: '',
        password: '',
        password_confirmation: '',
        name: '',
        image: ''
      },
      errors: []
    }
  },
  components: {
    GridVue,
    ErrosLista
  },
  created () {
    this.usuario = JSON.parse(sessionStorage.getItem('usuario'))
  },
  methods: {
    cadastrarConteudo () {
      axios.post('http://localhost:8000/api/cadastrarConteudo'), ({
        texto: this.conteudo,
        titulo: 'Teste',
        user: this.usuario.id,
        link: 'nulo'
      }).then(response => {
        console.log(response.data)
      }).catch(e => {
        this.errors.push(e)
      })
    }
  }
}
</script>

<style scoped>

</style>
