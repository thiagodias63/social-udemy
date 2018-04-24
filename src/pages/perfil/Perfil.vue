<template>
  <SiteTemplate>
    <span slot="menuEsquerdo">
      <img :src="usuario.image" alt="O dinheiro vem pra confundir o amor" style="width:100%">
    </span>
    <span slot="principal">
        <h2>Perfil</h2>
        <input type="text" v-model="usuario.name" placeholder="Nome:">
        <input type="email" v-model="usuario.email" placeholder="Email:">
        <input type="password" v-model="usuario.password" placeholder="Senha:">
        <input type="password" v-model="usuario.password_confirmation" placeholder="Confirme sua Senha:">
        <div class="file-field input-field">
          <div class="btn">
            <span>Selecione a foto</span>
            <input type="file" v-on:change="salvarImagem">
          </div>
          <div class="file-path-wrapper">
            <input class="file-path validate" type="text">
          </div>
        </div>
        <button class="btn red" v-on:click="alterar()">Alterar Perfil</button>
    </span>
  </SiteTemplate>
</template>

<script>
import SiteTemplate from '@/templates/SiteTemplate'
import axios from 'axios'

export default {
  name: 'Perfil',
  data () {
    return {
      usuario: {
        email: '',
        password: '',
        password_confirmation: '',
        name: '',
        image: ''
      }
    }
  },
  components: {
    SiteTemplate
  },
  created () {
    let usuarioAux = sessionStorage.getItem('usuario')
    if (usuarioAux) {
      this.usuario = JSON.parse(usuarioAux)
    } else {
      this.$route.push('/login')
    }
  },
  methods: {
    salvarImagem (e) {
      let arquivo = e.target.files || e.dataTransfer.files
      if (!arquivo.length) {
        return false
      }
      let reader = new FileReader()
      reader.onload = (e) => {
        this.usuario.image = e.target.result
      }
      reader.readAsDataURL(arquivo[0])
    },
    alterar () {
      axios.put('http://localhost:8000/api/atualizar', {
        email: this.usuario.email,
        password: this.usuario.password,
        password_confirmation: this.usuario.password_confirmation,
        name: this.usuario.name,
        image: this.usuario.image
      },
      {
        'headers': {
          'authorization': 'Bearer ' + this.usuario.token
        }
      }).then(response => {
        if (response.data.token) {
          sessionStorage.setItem('usuario', JSON.stringify(response.data))
          alert('Perfil atualizado')
        } else {
          let erros = ''
          for (let erro of Object.values(response.data)) { // transforma response.data em um array de valores
            erros += erro + ' '
          }
          alert(erros)
        }
      }).catch(e => {
        console.log(e)
        alert(e + 'Tente novamente mais tarde')
      })
    }
  }
}
</script>

<style scoped>

</style>
