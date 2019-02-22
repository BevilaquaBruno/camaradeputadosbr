<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col>
        <b-pagination size="md" :total-rows="513" v-model="currentPage" :per-page="10" />
      </b-col>
      <b-col></b-col>
    </b-row>
    <b-card-group deck>
      <div class="mt-4" v-for="dp of deputados" v-bind:key="dp.id">
        <b-card no-body class="overflow-hidden" style="width: 445px;">
          <b-row no-gutters>
            <b-col md="3">
              <b-img thumbnail :src="dp.urlFoto" class="rounded-0" />
            </b-col>
            <b-col md="8">
              <b-card-body>
                <span class="dpName">
                  {{dp.nome+' - '+dp.siglaPartido+'/'+dp.siglaUf}}
                </span>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>
      </div>
    </b-card-group>
    <b-row class="dpPagination-bottom">
      <b-col></b-col>
      <b-col>
        <b-pagination size="md" :total-rows="513" v-model="currentPage" :per-page="10" />
      </b-col>
      <b-col></b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  async asyncData () {
    return{
      deputados: {},
      links: {},
      currentPage: 1
    }
  },
  methods:{
    getAllDeputados(){
      let i = this;
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/deputados?itens=10&ordem=ASC&ordenarPor=nome&pagina="+i.currentPage)
      .then(
        function (response) {
          if(response.data.dados){
            i.deputados = response.data.dados;
            response.data.links.forEach(element => {
              i.links[element.rel] = element.href
            });
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar dados dos deputados: " + err);
      });
    }
  },
  watch: {
    currentPage: function (newPg, oldPg) {
      this.getAllDeputados();
    }
  },
  mounted(){
    this.getAllDeputados();
  }
}
</script>
<style scoped>
.dpName{
  font-weight: 500;
}

.dpPagination-bottom{
  padding-top: 35px;
}
</style>
