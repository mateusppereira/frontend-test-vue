<template>
  <section class="container">
    <div class="top">
      <ul class="top__breadcumb">
        <li class="top__breadcumb__item">
          <span class="text text--primary text--sm text--bold">Home</span>
        </li>
        <li class="top__breadcumb__item">
          <span class="text text--primary text--sm text--bold">Minha conta</span>
        </li>
        <li class="top__breadcumb__item top__breadcumb__item--selected">
          <span class="text text--primary text--sm text--bold">Bolsas favoritas</span>
        </li>
      </ul>
      <div class="top__title">
        <h1 class="text text--bold text--xxlg">Bolsas favoritas</h1>
        <span class="text">Adicione bolsas de cursos e faculdades de seu interesse e receba atualizações com as melhores ofertas disponíveis.</span>
      </div>
      <div class="top__filters">
        <div
          :class="`top__filters__item ${period === '' && 'top__filters__item--selected'}`"
          @click="period = ''"
        >
          <span class="text text--bold">Todos os semestres</span>
        </div>
        <div
        :class="`top__filters__item ${period === '2019.2' && 'top__filters__item--selected'}`"
          @click="period = '2019.2'"
        >
          <span class="text text--bold">2° semestre de 2019</span>
        </div>
        <div
          :class="`top__filters__item ${period === '2020.1' && 'top__filters__item--selected'}`"
          @click="period = '2020.1'"
        >
          <span class="text text--bold">1° semestre de 2020</span>
        </div>
      </div>
    </div>
    <div class="favorites">
      <div class="favorites__item favorites__item--new" @click="changeModalState()">
        <div>
          <i class="fas fa-info-circle fa-10x" />
          <span class="text text--bold">Adicionar bolsa</span>
          <span class="text text--center">Clique para adicionar bolsas de curso do seu interesse</span>
        </div>
      </div>
      <div
        v-for="favorite in favorites"
        :key="favorite.id"
        :class="`favorites__item ${!checkVisible(favorite.enrollment_semester) && 'favorites__item--hidden'}`"
      >
        <img class="favorites__item__logo" :src="favorite.university.logo_url" />
        <div class="favorites__item__infos">
          <span class="text text--bold text--center">{{favorite.university.name}}</span>
          <span class="text text--bold text--center text--primary">{{favorite.course.name}}</span>
          <div class="favorites__item__infos favorites__item__infos--row">
            <span class="text">{{favorite.university.score}}</span>
            <span
              v-for="i in Math.round(favorite.university.score)"
              :key="i"
              class="text"
            >
              X
            </span>
          </div>
        </div>
        <div class="favorites__item__infos">
          <span class="text text--bold text--center">{{favorite.course.kind}} - {{favorite.course.shift}}</span>
          <span class="text text--sm text--center">Ínicio das aulas em {{favorite.start_date}}</span>
        </div>
        <div class="favorites__item__infos">
          <span class="text text--bold text--center">
            Mensalidade com o Quero Bolsa
          </span>
          <span class="text text--sm text--line-through text--center">
            R$ {{favorite.full_price}}
          </span>
          <span class="text text--lg text--center text--green text--bold">
            R$ {{favorite.full_price}}<span class="text text--dark">/mês</span>
          </span>
        </div>
        <div class="favorites__buttons">
          <button class="button button--outline" @click="removeFavorite(favorite.id)">
            Excluir
          </button>
          <button class="button button--yellow">
            Ver oferta
          </button>
        </div>
      </div>
    </div>
    <Modal ref="modal" @addFavoritesById="addFavoritesById" />
  </section>
</template>
<style lang="scss" scoped>
  @import "../../assets/_text.scss";
  @import "../../assets/_button.scss";
  @import "./Body.scss";
</style>
<script>
import Axios from 'axios';
import Modal from './components/modal/Modal';
export default {
  name: 'Body',
  components: { Modal },
  data () {
    return {
      favorites: [],
      scholarships: [],
      period: ''
    }
  },
  mounted () {
    const favorites = JSON.parse(sessionStorage.getItem('favorites')) || [];
    Axios.get('https://testapi.io/api/redealumni/scholarships').then(({ data }) => {
      const withIds = data.map((item, i) => ({
        id: i,
        ...item,
      }))
      this.favorites = favorites;
      this.scholarships = withIds;
      this.$refs.modal.dataLoaded(favorites, withIds);
    })
  },
  methods: {
    checkVisible (period) {
      if (this.period) {
        return this.period === period;
      }
      return true;
    },
    changeModalState () {
      this.$refs.modal.toggleModal();
    },
    removeFavorite (id) {
      this.favorites = this.favorites.filter(item => item.id !== id);
      this.$refs.modal.dataLoaded(this.favorites, this.scholarships);
      sessionStorage.setItem('favorites', JSON.stringify(this.favorites));
    },
    addFavoritesById (ids) {
      const favorites = this.scholarships.filter(item => ids.indexOf(item.id) >= 0);
      this.favorites = favorites;
      sessionStorage.setItem('favorites', JSON.stringify(this.favorites));
    }
  }
}
</script>

