<template>
  <div class="container">
    <h2 v-if="error.statusCode === 404">Página não encontrada :c</h2>
    <h2 v-else>Um erro ocorreu :c</h2>
  </div>
</template>

<script>
export default {
  props: ['error'],
  layout: 'default' // you can set a custom layout for the error page
}
</script>