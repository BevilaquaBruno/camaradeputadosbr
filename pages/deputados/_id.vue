<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col>
        <div v-if="deputado != null">
          <div class="divData">
            <img :src="deputado.ultimoStatus.urlFoto" :alt="deputado.nomeCivil">
          </div>
        </div>
      </b-col>
      <b-col></b-col>
    </b-row>
  </div>
</template>
<script>
export default {
  async asyncData({ params }){
    return{
      id: params.id,
      deputado: null
    }
  },
  methods:{
    getAllAboutDeputado(){
      console.log('a');
      let i = this;
      i.$axios.setHeader("Accept", "application/json");
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/deputados/"+i.id)
      .then(
        function (response) {
          if (response.data) {
            i.deputado = response.data.dados
            console.log(response.data.dados);
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar dados do deputado: " + err);
      });
    }
  },
  mounted(){
    this.getAllAboutDeputado();
  }
}
</script>

<style scoped>
.divData{
  border: 1px solid lightgrey;
  padding: 5px;
  border-radius: 5px;
}
</style>
