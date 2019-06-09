<template>
  <div id="app">
    <Header />
    <Body />
    <Footer />
  </div>
</template>
<style lang="scss">
  @import './assets/variables.scss';
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    outline: 0;
  }
  html, body, #app {
    overflow-x: hidden;
    font-family: 'Lato', sans-serif;

    color: $textBlack;
    font-size: 62.5%;
    @media (max-width: $mobile) {
      font-size: 52.5%;
    }
  }

  #app {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: stretch;
  }
</style>


<script>
import Header from './components/header/Header.vue'
import Footer from './components/footer/Footer.vue'
import Body from './components/body/Body.vue'

export default {
  name: 'app',
  components: {
    Header,
    Body,
    Footer
  }
}
</script>