<template>
  <div>
    <ul v-if="errors && errors.length" class="collection with-header">
      <li class="collection-header grey">Erros</li>
      <li v-for="error of errors" :key="error.message" class="collection-item red">
        {{errors.message}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ErrosLista',
  props: ['errors'],
  data () {
    return {

    }
  }
}
</script>

<style scoped>

</style>
