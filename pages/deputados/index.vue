<template>
  <div>
    <b-row>
      <b-col offset="4">
        <h2 class="dpTitle">Lista de Deputados</h2>
      </b-col>
      <b-col></b-col>
    </b-row>
    <b-row>
      <b-col offset="4">
        <b-pagination size="md" :total-rows="total_rows" v-model="currentPage" :per-page="10" />
      </b-col>
      <b-col>
        <b-button v-b-toggle.collapse1 variant="primary">
          <i class='uil uil-search-alt'></i>
        </b-button>
      </b-col>
    </b-row>
    <b-collapse id="collapse1" class="mt-2">
      <b-row>
        <b-col cols="1">
          <label for="dpNome">Nome: </label>
        </b-col>
        <b-col cols="5">
          <b-form-input id="dpNome" size="sm" class="mr-sm-2" type="text" placeholder="Nome do deputado" v-model="nome" />
        </b-col>
        <b-col cols="1">
          <label for="dpUf">UF: </label>
        </b-col>
        <b-col cols="3">
          <b-form-select v-model="sigla_selected" :options="siglas" size="sm" />
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="1">
          <label for="dpPartido">Partido: </label>
        </b-col>
        <b-col b-cols="4">
          <b-form-select v-model="partido_selected" :options="partidos" size="sm" />
        </b-col>
        <b-col cols="7"></b-col>
      </b-row>
    </b-collapse>
    <div v-if="deputados.length == 0">
      <b-alert show variant="info">Sem mais dados nas páginas seguintes !</b-alert>
    </div>
    <div v-else>
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
    </div>
    <b-row class="dpPagination-bottom">
      <b-col offset="4">
        <b-pagination size="md" :total-rows="total_rows" v-model="currentPage" :per-page="10" />
      </b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  async asyncData () {
    return{
      deputados: {},
      currentPage: 1,
      total_rows: 513,
      nome: "",
      nomeUrl:"",
      sigla_selected: 0,
      siglas:[],
      siglaUrl:"",
      partido_selected: 0,
      partidos: [],
      partidoUrl:""
    }
  },
  methods:{
    getAllDeputados(){
      let i = this;
      i.$axios.setHeader("Accept", "application/json");
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/deputados?itens=10&ordem=ASC&ordenarPor=nome&pagina="+i.currentPage+""+i.nomeUrl+""+i.siglaUrl+""+i.partidoUrl)
      .then(
        function (response) {
          if(response.data.dados){
            i.deputados = response.data.dados;
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar dados dos deputados: " + err);
      });
    },
    getAllSiglasUf(){
      let i = this;
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/referencias/deputados/siglaUF")
      .then(
        function (response) {
          if(response.data.dados){
            i.siglas.push({ value: 0, text:'Todos os estados' });
            response.data.dados.forEach(element =>{
              i.siglas.push({ value: element.sigla, text: element.nome + ' - ' + element.sigla });
            });
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar siglas dos estados: " + err);
      });
    },
    getAllSiglasPartidos(){
      let i = this;
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/partidos?ordem=ASC&ordenarPor=sigla")
      .then(
        function (response) {
          if(response.data.dados){
            i.partidos.push({ value: 0, text:'Todos os partidos' });
            response.data.dados.forEach(element =>{
              i.partidos.push({ value: element.sigla, text: element.nome + ' - ' + element.sigla });
            });
          }
        }
      ).catch(function (err) {
        console.error("Erro ao encontrar siglas dos estados: " + err);
      });
    }
  },
  watch: {
    currentPage: function (newPg, oldPg) {
      this.getAllDeputados();
    },
    nome: function (newName, oldName) {
      this.nomeUrl = "&nome="+newName;
      this.currentPage = 1;
      this.getAllDeputados();
    },
    sigla_selected: function (newSigla, oldSigla) {
      if (newSigla == 0) {
        this.siglaUrl = "";
      }else{
        this.siglaUrl = "&siglaUf="+newSigla;
      }
      this.currentPage = 1;
      this.getAllDeputados();
    },
    partido_selected: function (newPartido, oldPartido) {
      if (newPartido == 0) {
        this.partidoUrl = "";
      }else{
        this.partidoUrl = "&siglaPartido="+newPartido;
      }
      this.currentPage = 1;
      this.getAllDeputados();
    }
  },
  mounted(){
    this.getAllDeputados();
    this.getAllSiglasUf();
    this.getAllSiglasPartidos();
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
.dpTitle{
  margin-bottom: 25px;
}
</style>
