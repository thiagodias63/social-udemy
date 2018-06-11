<template>
  <div class="row">
    <div class="card">
      <div class="card-content">

        <div class="row valign-wrapper">
          <GridVue tamanho="1">
            <img :src="perfil" :alt="nome" class="circle responsive-img"> <!-- notice the "circle" class -->
          </GridVue>
          <GridVue tamanho="11">
            <span class="black-text">
              <strong>{{ nome }}</strong> - <small> {{ data }}</small>
            </span>
          </GridVue>
        </div>

        <slot></slot>

      </div>
      <div class="card-action">
        <p>
          <a @click="curtir(id)">
            <i class="material-icons red-curtir">{{ curtiu }}</i>{{ total_curtidas }}
          </a>
          <a @click="abreComentario()">
            <i class="material-icons">insert_comment</i> {{ listaComentarios.length }}
          </a>
        </p>
        <p class="right-align"  v-if="exibirComentario">
          <input type="text" placeholder="Comentar" v-model="comentario_texto">
          <button v-if="comentario_texto" @click="comentar(id)" type="button" class="btn waves-effect waves-light orange"><i class="material-icons">send</i></button>
        </p>
        <p v-if="exibirComentario">
          <ul class="collection">
            <li class="collection-item avatar" v-for="comentario in listaComentarios" :key="comentario.id">
              <img :src="comentario.user.image" alt="" class="circle">
              <span class="title"> {{comentario.user.name }} <small>- {{ comentario.data }}</small></span>
              <p>
                {{ comentario.texto }}
              </p>
            </li>
          </ul>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import GridVue from '@/components/layout/GridVue'

export default {
  name: 'CardConteudoVue',
  props: ['id', 'perfil', 'nome', 'data', 'totalCurtidas', 'totalComentarios', 'comentarios', 'curtiuConteudo'],
  data () {
    return {
      curtiu: this.curtiuConteudo ? 'favorite' : 'favorite_border',
      total_curtidas: this.totalCurtidas,
      total_comentarios: this.totalComentarios,
      listaComentarios: this.comtarios || [],
      exibirComentario: false,
      comentario_texto: ''
    }
  },
  components: {
    GridVue
  },
  methods: {
    curtir (id) {
      this.$http.put(this.$url + '/api/conteudo/curtir/' + id, {}, {
        'headers': {
          'authorization': 'Bearer ' + this.$store.getters.getToken
        }
      }).then(response => {
        if (response.data.status) {
          this.total_curtidas = response.data.curtidas
          this.$store.commit('setConteudosLinhaTempo', response.data.lista.conteudos.data)
          if (this.curtiu === 'favorite_border') {
            this.curtiu = 'favorite'
          } else {
            this.curtiu = 'favorite_border'
          }
        } else {
          alert(response.data.erros)
        }
      }).catch(e => {
        this.errors.push(e)
      })
    },
    abreComentario () {
      this.exibirComentario = !this.exibirComentario
    },
    comentar (id) {
      if (!this.comentario_texto) {
        return false
      }
      this.$http.put(this.$url + '/api/conteudo/comentar/' + id, {
        texto: this.comentario_texto
      }, {
        'headers': {
          'authorization': 'Bearer ' + this.$store.getters.getToken
        }
      }).then(response => {
        if (response.data.status) {
          this.$store.commit('setConteudosLinhaTempo', response.data.lista.conteudos.data)
          this.comentario_texto = ''
        } else {
          alert(response.data.erros)
        }
      }).catch(e => {
        this.errors.push(e)
      })
    }
  }
}
</script>

<style scoped>
.red-curtir{
  color:#FA0014
}
a {
  cursor: pointer;
}
</style>
