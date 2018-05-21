<template>
  <div class="row">
    <GridVue class="input-field" tamanho="12">
      <input type="text" v-model="conteudo.titulo">
      <textarea v-if="conteudo.titulo" placeholder="Conteúdo" v-model="conteudo.texto" class="materialize-textarea"></textarea>
      <label>O que está acontecendo?</label>
      <input v-if="conteudo.titulo && conteudo.texto" type="text" placeholder="Link" v-model="conteudo.link">
      <input v-if="conteudo.titulo && conteudo.texto" type="text" placeholder="Url da imagem" v-model="conteudo.imagem">
    </GridVue>
    <p class="right-align">
      <button v-if="conteudo.titulo && conteudo.texto" @click="cadastrarConteudo()" class="btn waves-effect waves-light">Publicar</button>
    </p>
    <ErrosLista :errors="errors"/>
  </div>
</template>

<script>
import GridVue from '@/components/layout/GridVue'
import ErrosLista from '@/components/layout/ErrosLista'

export default {
  name: 'PublicarConteudoVue',
  data () {
    return {
      conteudo: {
        titulo: '',
        texto: '',
        link: '',
        imagem: ''
      },
      errors: []
    }
  },
  components: {
    GridVue,
    ErrosLista
  },
  created () {
  },
  methods: {
    cadastrarConteudo () {
      this.$http.post(this.$url + '/api/conteudo/cadastrar', {
        titulo: this.conteudo.titulo,
        texto: this.conteudo.texto,
        link: this.conteudo.link,
        imagem: this.conteudo.imagem
      },
      {
        'headers': {
          'authorization': 'Bearer ' + this.$store.getters.getToken
        }
      }).then(response => {
        if (response.data.status) {
          // console.log(response.data)
          this.conteudo = {
            titulo: '',
            texto: '',
            link: '',
            imagem: ''
          }
          this.$store.commit('setConteudosLinhaTempo', response.data.conteudos.data)
        }
      }).catch(e => {
        this.errors.push(e)
      })
    }
  }
}
</script>

<style scoped>

</style>
