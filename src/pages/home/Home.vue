<template>
  <span>
    <SiteTemplate>
      <span slot="menuEsquerdo">
        <div class="row valign-wrapper">
          <GridVue tamanho="4">
            <img :src="usuario.image" :alt="usuario.name" class="circle responsive-img"> <!-- notice the "circle" class -->
          </GridVue>
          <GridVue tamanho="8">
            <span class="black-text">
              <h5>{{usuario.name}}</h5>
              {{usuario.email}}
            </span>
          </GridVue>
        </div>
      </span>
      <span slot="principal">
        <PublicarConteudoVue />
        <ErrosLista :errors="errors"/>
        <span v-if="posts && posts.length">
          <span v-for="post of posts">
            <CardConteudoVue :perfil="usuario.image"
            :nome="usuario.name"
            :data="post.data">
            <CardDetalheVue imagem="https://i.ytimg.com/vi/NJi6ts5D5s8/maxresdefault.jpg" :texto="post.texto" :titulo="post.titulo"></CardDetalheVue>
            </CardConteudoVue>
          </span>
        </span>
      </span>
    </SiteTemplate>
  </span>
</template>

<script>
import SiteTemplate from '@/templates/SiteTemplate'
import GridVue from '@/components/layout/GridVue'
import ErrosLista from '@/components/layout/ErrosLista'
import CardConteudoVue from '@/components/social/CardConteudoVue'
import PublicarConteudoVue from '@/components/social/PublicarConteudoVue'
import CardDetalheVue from '@/components/social/CardDetalheVue'
import axios from 'axios'

export default {
  name: 'Home',
  data () {
    return {
      usuario: {
        email: '',
        password: '',
        password_confirmation: '',
        name: '',
        image: ''
      },
      posts: [],
      errors: []
    }
  },
  created () {
    let usuarioAux = sessionStorage.getItem('usuario')
    if (usuarioAux) {
      this.usuario = JSON.parse(usuarioAux)
      this.listarConteudos()
    } else {
      this.$route.push('/login')
    }
  },
  components: {
    SiteTemplate,
    GridVue,
    CardConteudoVue,
    CardDetalheVue,
    PublicarConteudoVue,
    ErrosLista
  },
  methods: {
    listarConteudos () {
      axios.post('http://localhost:8000/api/listarConteudo', {
        user: this.usuario.id
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
