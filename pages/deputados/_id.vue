<template>
  <div>
    <b-row>
      <b-col>
        <div v-if="deputado != null" style="text-align:center">
          <b-img :src="deputado.ultimoStatus.urlFoto" fluid rounded
            v-bind="{ blank: false, blankColor: '#777', width: 150, height: 150, class: 'm1' }"
            :alt="deputado.nomeCivil">
          </b-img>
          <div class="h2">{{ deputado.nomeCivil }} - {{ deputado.ultimoStatus.siglaPartido }} / {{ deputado.ultimoStatus.siglaUf }}</div>
          <b-row>
            <b-col>
              <b-card
                :title="'Dados do deputado - ' + deputado.ultimoStatus.situacao + ' ' + deputado.ultimoStatus.condicaoEleitoral "
                tag="article"
                class="mt-pc"
                style="max-width: 25rem;max-height: 30rem"
              >
              <b-list-group>
                <b-list-group-item><strong>CPF:</strong> {{ deputado.cpf }}</b-list-group-item>
                <b-list-group-item><strong>Sexo:</strong> {{ sexo }}</b-list-group-item>
                <b-list-group-item><strong>Nasc:</strong> {{ deputado.municipioNascimento }} - {{ deputado.ufNascimento }}</b-list-group-item>
                <b-list-group-item><strong>Escolaridade:</strong> {{ deputado.escolaridade }}</b-list-group-item>
                <b-list-group-item><strong>Nascimento:</strong> {{ nascimento }}</b-list-group-item>
                <b-list-group-item>
               <div v-if="deputado.urlWebsite !== null" >
                    <b-link :href="deputado.urlWebsite">{{ deputado.urlWebsite }}</b-link>
                  </div>
                  <div v-else>Parlamentar sem website</div>
                </b-list-group-item>
              </b-list-group>
              </b-card>
            </b-col>
            <b-col>
              <b-card
                title="Dados do gabinete"
                tag="article"
                class="mt-pc"
                style="max-width: 25rem;min-height: 25.35rem;>"
              >
              <b-list-group>
                <b-list-group-item><strong>Nome:</strong> {{ deputado.ultimoStatus.gabinete.nome }}</b-list-group-item>
                <b-list-group-item><strong>Prédio:</strong> {{ deputado.ultimoStatus.gabinete.predio }}</b-list-group-item>
                <b-list-group-item><strong>Sala:</strong> {{ deputado.ultimoStatus.gabinete.sala }}</b-list-group-item>
                <b-list-group-item><strong>Andar:</strong> {{ deputado.ultimoStatus.gabinete.andar }}</b-list-group-item>
                <b-list-group-item><strong>Telefone:</strong> {{ deputado.ultimoStatus.gabinete.telefone }}</b-list-group-item>
                <b-list-group-item><strong>Email:</strong> <b-link :href="'mailto:'+deputado.ultimoStatus.gabinete.email">{{ deputado.ultimoStatus.gabinete.email }}</b-link></b-list-group-item>
              </b-list-group>
              </b-card>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <b-card
                title="Redes Sociais"
                tag="article"
              >
              <div v-if="deputado.redesocial == undefined">
                <h5>Deputado sem rede social informada.</h5>
              </div>
              <div v-else>
                <h5> Em breve ! </h5>
              </div>
              </b-card>
            </b-col>
          </b-row>
        </div>
      </b-col>
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
  computed: {
    sexo: function () {
      return (this.deputado.sexo == 'M') ? 'Masculino' : 'Feminino' ;
    },
    nascimento: function () {
      var date = this.deputado.dataNascimento.split('-');
      return (date[2]+'/'+date[1]+'/'+date[0])
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
@media (min-width: 992px){
  .mt-pc{
    margin-left: 5%;
  }
}
</style>
