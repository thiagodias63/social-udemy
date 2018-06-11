<template>
  <span>
    <div v-if="imagem && imagem != '#'" class="card-image">
      <a v-if="link && link != '#'" :href="link" target="_blank">
        <img :src="imagem">
        <span class="card-title">{{ titulo || ''}}</span>
      </a>
      <span v-else>
        <img :src="imagem">
        <span class="card-title">{{ titulo || ''}}</span>
      </span>
    </div>

    <div class="card-content">
      <span v-if="!imagem || imagem == '#'">
        <span class="card-title">{{ titulo || ''}}</span>
      </span>
      <p>{{ texto }}.</p>
      <a v-if="link && link != '#'" :href="link" target="_blank">
        Confira aqui
      </a>
    </div>
  </span>
</template>

<script>

export default {
  name: 'CardDetalheVue',
  props: ['imagem', 'link', 'titulo', 'texto'],
  data () {
    return {
    }
  }
}
</script>

<style scoped>

</style>
