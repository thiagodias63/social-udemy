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
        <CardConteudoVue v-for="conteudo in listaConteudosLinhaTempo"
          :key="conteudo.id"
          :perfil="conteudo.user.image"
          :nome="conteudo.user.name"
          :data="conteudo.user.created_at">
          <CardDetalheVue
          :imagem="conteudo.image"
          :titulo="conteudo.titulo"
          :texto="conteudo.texto">
          </CardDetalheVue>
        </CardConteudoVue>
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
      conteudos: [],
      errors: []
    }
  },
  created () {
    let usuarioAux = this.$store.getters.getUsuario
    if (usuarioAux) {
      this.usuario = this.$store.getters.getUsuario
      this.listarConteudos()
    } else {
      this.$router.push('/login')
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
      this.$http.get(this.$url + '/api/conteudo/listar', {
        'headers': {
          'authorization': 'Bearer ' + this.$store.getters.getToken
        }
      }).then(response => {
        if (response.data.status) {
          console.log(response.data.conteudos.data)
          // this.conteudos = response.data.conteudos.data
          this.$store.commit('setConteudosLinhaTempo', response.data.conteudos.data)
          // this.conteudos = this.$store.getters.getConteudosLinhaTempo
        }
      }).catch(e => {
        this.errors.push(e)
      })
    }
  },
  computed: {
    listaConteudosLinhaTempo () {
      return this.$store.getters.getConteudosLinhaTempo
    }
  }
}
</script>

<style scoped>

</style>
