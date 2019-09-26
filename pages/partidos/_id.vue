<template>
  <div>
    <div v-if="partido != null">
      <b-img :src="partido.urlLogo" fluid rounded
            v-bind="{ blank: false, blankColor: '#777', class: 'm1' }"
            :alt="partido.nome">
          </b-img>
          <!-- <div class="h2">{{ deputado.nomeCivil }} -  <nuxt-link :to="'/partidos/'+deputado.ultimoStatus.siglaPartido"> {{ deputado.ultimoStatus.siglaPartido }} </nuxt-link> / {{ deputado.ultimoStatus.siglaUf }}</div> -->
    </div>
    <div v-else>
      Problemas ao pegar o partido
    </div>
  </div>
</template>
<script>
export default {
  async asyncData({ params }){
    return{
      id: params.id,
      partido: null
    }
  },
  methods:{
    getAllAboutPartido(){
      let i = this;
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/partidos/"+this.id)
      .then(
        function (response) {
          if(response.data.dados){
            console.log(response.data.dados);
            i.partido = response.data.dados;
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar partidos: " + err);
      });
    }
  },
  mounted(){
    this.getAllAboutPartido();
  }
}
</script>

<style scoped>
@media (min-width: 992px){
  .mt-pc{
    margin-left: 5%;
  }
}
</style>
