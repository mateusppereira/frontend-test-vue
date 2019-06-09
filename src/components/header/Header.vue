<template>
  <header class="container">
    <div class="top">
      <div class="top__left">
        <div class="top__left__how-it-works">
          <i class="fas fa-info-circle fa-6x" />
          <span class="text text--bold text--primary">Como funciona</span>
        </div>
        <div class="top__left__contacts">
          <img class="top__left__contacts--img" :src="wppImg">
          <span class="text text--bold text--primary">Como funciona</span>
        </div>
      </div>
      <img  class="top__logo" src="http://izabelahendrix.edu.br/noticias/2018/quero-bolsa-entrevista-reitor-e-psicologa-do-izabela-sobre-saude-mental-de-universitarios/@@images/ae57c16e-32df-4f50-94ed-fbfdee916748.png" />
      <div class="top__right">
        <div class="top__right__account">
          <span class="text text--bold text--primary">Nome Sobrenome</span>
          <i class="fas fa-user-circle fa-6x"/>
        </div>
      </div>
    </div>
    <nav class="nav">
      <ul class="nav__links">
        <li class="nav__links__item nav__links__item--fixed">
          <span class="text text--white text--bold">Minha conta</span>
        </li>
        <li class="nav__links__item">
          <span class="text text--white text--bold text--sm">Pré-matriculas</span>
        </li>
        <li class="nav__links__item nav__links__item--selected">
          <span class="text text--white text--bold text--sm">Bolsas favoritas</span>
        </li>
        <li class="nav__links__item nav__links__item--mobile">
          <span class="text text--white text--bold text--sm">Menu</span>
          <i class="fas fa-chevron-down fa-3x" />
        </li>
      </ul>
    </nav>
  </header>
</template>
<style lang="scss" scoped>
  @import "../../assets/_text.scss";
  @import "./_Header.scss";
</style>

<script>
import whatsapp from '../../assets/whatsapp1.png';
export default {
  name: 'Header',
  data () {
    return {
      wppImg: whatsapp
    }
  }
}
</script>

