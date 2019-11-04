<template>
  <div>
    <div v-if="partido != null">
      <b-row>
        <b-col class="text-right ">
          <div v-if="partido.urlLogo != ''">
            <b-img :src="partido.urlLogo" fluid rounded
              v-bind="{ blank: false, blankColor: '#777', class: 'm1' }"
              :alt="partido.nome">
            </b-img>
          </div>
        </b-col>
        <b-col cols="10">
          <div class="h2 text-left">{{ partido.nome }}</div>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="1"></b-col>
        <b-col cols="3">
          <div v-if="partido.numeroEleitoral != null">
            Nº Eleitoral: {{ partido.numeroEleitoral }}
          </div>
          <div v-else>
            Sem Número Eleitoral Informado
          </div>
        </b-col>
        <b-col cols="4">
          <div v-if="partido.urlWebSite != null">
            WebSite: {{ partido.urlWebSite }}
          </div>
          <div v-else>
            Sem WebSite Informado
          </div>
        </b-col>
        <b-col cols="4">
          <div v-if="partido.urlFacebook != null">
            Facebook: {{ partido.urlFacebook }}
          </div>
          <div v-else>
            Sem Facebook Informado
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <p class="h4">Status: </p>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="1"></b-col>
        <b-col cols="3">
          Situação: {{ partido.status.situacao }}
        </b-col>
        <b-col cols="3">
          Total Posse: {{ partido.status.totalPosse }}
        </b-col>
        <b-col cols="3">
          Total Membros: {{ partido.status.totalMembros }}
        </b-col>
      </b-row>
      <hr>
      <b-row>
        <b-col>
          <p class="h4">Líder do Partido </p>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="1"></b-col>
        <b-col cols="4">
          <div v-if="partido.status.lider.nome != null">
            <a :href="'/deputados/'+deputadoId">
              <b-img width="100" thumbnail :src="partido.status.lider.urlFoto" class="rounded-0" />
            </a>
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="1"></b-col>
        <b-col cols="4">
          <div v-if="partido.status.lider.nome != null">
            <nuxt-link :to="'/deputados/'+deputadoId">
              <span class="dpName">
                {{partido.status.lider.nome+' - '+partido.status.lider.siglaPartido+'/'+partido.status.lider.uf}}
              </span>
            </nuxt-link>
          </div>
          <div v-else>
            Sem Líder do Partido Informado
          </div>
        </b-col>
      </b-row>
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
      partido: null,
      deputadoId: null
    }
  },
  methods:{
    getAllAboutPartido(){
      let i = this;
      i.$axios.get("https://dadosabertos.camara.leg.br/api/v2/partidos/"+this.id)
      .then(
        function (response) {
          if(response.data.dados){
            i.partido = response.data.dados;
            let arr = response.data.dados.status.lider.uri.split('/');
            i.deputadoId = arr[arr.length-1];
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
.dpName{
  font-weight: 500;
}
</style>
