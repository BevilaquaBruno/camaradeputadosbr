<template>
  <div>

    <b-row>
      <b-col offset="4">
        <h2 class="ptTitle">Lista de Partidos</h2>
      </b-col>
    </b-row>
    <b-row>
      <b-col cols="12" sm="6">
        <b-form-group id="ptNome" label="Pesquisar sigla:">
          <b-form-input label id="ptNome" size="sm" class="mr-sm-2" type="text" placeholder='Para mais de 1(um) partido utilize "," para separar ' v-model="sigla" />
        </b-form-group>
      </b-col>
    </b-row>
    <div v-if="partidos.length == 0">
      <b-alert show variant="info">Sem Partidos !</b-alert>
    </div>
    <div v-else>
      <b-card-group deck>
        <div class="mt-2" v-for="pt of partidos" v-bind:key="pt.id">
          <b-card no-body class="overflow-hidden" style="width: 445px;">
            <b-row no-gutters>
              <b-col md="12">
                <b-card-body>
                  <nuxt-link :to="'/partidos/'+pt.id">
                    <span class="ptName">
                      {{ pt.nome+' - '+pt.sigla }}
                    </span>
                  </nuxt-link>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </div>
      </b-card-group>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData () {
    return{
      sigla: "",
      siglaUrl: "",
      partidos: {}
    }
  },
  methods:{
    getAllSiglasPartidos(){
      let i = this;
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/partidos?ordem=ASC&ordenarPor=sigla&itens=100"+this.siglaUrl)
      .then(
        function (response) {
          if(response.data.dados){
            i.total_rows = response.data.dados.length;
            i.partidos = response.data.dados;
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar partidos: " + err);
      });
    }
  },
  watch: {
    currentPage: function (newPg, oldPg) {
      this.getAllSiglasPartidos();
    },
    sigla: function (newSigla, oldSigla) {
      this.siglaUrl = "&sigla="+newSigla;
      this.currentPage = 1;
      this.getAllSiglasPartidos();
    }
  },
  mounted(){
    this.getAllSiglasPartidos();
  }
}
</script>


<style scoped>
  .ptName{
    font-weight: 500;
  }

  .ptPagination-bottom{
    padding-top: 35px;
  }
  .ptTitle{
    margin-bottom: 25px;
  }
</style>