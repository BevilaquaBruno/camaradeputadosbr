<template>
  <div>
    <div class="myNavbar">
      <b-navbar type="dark" variant="primary" toggleable="lg">
        <b-navbar-toggle target="nav_dropdown_collapse"></b-navbar-toggle>
        <b-collapse is-nav id="nav_dropdown_collapse">
          <b-navbar-nav>
            <b-nav-item href="/">Início</b-nav-item>
            <b-nav-item href="/deputados">Deputados</b-nav-item>
            <b-nav-item href="/partidos">Partidos</b-nav-item>
            <b-nav-item href="/blocos">Blocos</b-nav-item>
            <b-nav-item href="/eventos">eventos</b-nav-item>
            <b-nav-item href="/legislaturas">Legislaturas</b-nav-item>
            <b-nav-item href="/proposicoes">Proposições</b-nav-item>
            <b-nav-item href="/orgaos">Órgãos</b-nav-item>
            <b-nav-item href="/referencias">Referências</b-nav-item>
          </b-navbar-nav>
          <b-navbar-nav class="ml-auto">
            <b-navbar-brand href="/quemsomos" right>Quem Somos</b-navbar-brand>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
    <b-container>
      <b-row>
        <b-col cols="1"></b-col>
        <b-col>
          <nuxt />
        </b-col>
        <b-col cols="1"></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<style>
.myNavbar{
  margin-bottom: 2vh;
}

</style>