<template>
  <footer class="container">
    <div class="contacts">
      <div class="contacts__item contacts__item--main">
        <i class="fas fa-phone-volume fa-7x" />
        <div class="contacts__item__infos">
          <span class="text text--white text--bold">0800 123 2222</span>
          <span class="text text--white">Seg-Sex 08h-22h</span>
        </div>
      </div>
      <div class="contacts__item">
        <i class="fas fa-comments fa-7x" />
        <div class="contacts__item__infos">
          <span class="text text--white text--bold">Chat ao vivo</span>
          <span class="text text--white">Seg-Sex 08h-22h</span>
        </div>
        <span class="contacts__item__caption">Chat</span>
      </div>
      <div class="contacts__item">
        <i class="fas fa-envelope fa-7x" />
        <div class="contacts__item__infos">
          <span class="text text--white text--bold">Mande um e-mail</span>
          <span class="text text--white">Respondemos rapidinho</span>
        </div>
        <span class="contacts__item__caption">E-mail</span>
      </div>
      <div class="contacts__item">
        <i class="fas fa-info-circle fa-7x" />
        <div class="contacts__item__infos">
          <span class="text text--white text--bold">Central de ajuda</span>
          <span class="text text--white">Encontre todas respostas</span>
        </div>
        <span class="contacts__item__caption">Ajuda</span>
      </div>
    </div>
    <div class="bottom">
      <span class="text text--white text--sm">Feito com <i class="fas fa-heart" /> pela Quero Educação</span>
    </div>
  </footer>
</template>
<style lang="scss" scoped>
  @import "../../assets/_text.scss";
  @import "./Footer.scss";
</style>
<script>
export default {
  name: 'Footer'
}
</script>

