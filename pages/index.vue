<template>
  <div class="text_centered">
    <h1 class="title">
      camaradeputadosbr - {{ user.name }}
    </h1>
    <h2 class="subtitle">
      This project was maded with Nuxt.js
    </h2>
    <div class="links">
      <b-button variant="success">
        <a href="https://nuxtjs.org/" target="_blank" style="color: white;">
          Documentation Nuxt
        </a>
      </b-button>
      <b-button variant="secondary">
        <a href="https://github.com/nuxt/nuxt.js" target="_blank" style="color: white;">
          GitHub Nuxt
        </a>
      </b-button>
    </div>
    <h2 class="subtitle">
      And with bootstrap-vue
    </h2>
    <div class="links">
      <b-button variant="primary">
        <a href="https://bootstrap-vue.js.org/" target="_blank" style="color: white;">
          Documentation bootstrap-vue
        </a>
      </b-button>
      <b-button variant="secondary">
        <a href="https://github.com/bootstrap-vue/bootstrap-vue" target="_blank" style="color: white;">
          GitHub bootstrap-vue
        </a>
      </b-button>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData () {
    return {
      user: {
        name: 'Bruno Bevilaqua',
      }
    }
  }
}
</script>

<style>

.links{
  margin-bottom: 1vh;
  margin-top: 1vh;
}

.text_centered{
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

</style>
