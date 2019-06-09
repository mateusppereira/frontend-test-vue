<template>
  <div :class="`background ${!modalState && 'background--hidden' }`">
    <div :class="`content ${!modalState && 'content--hidden' }`">
      <div class="top">
        <h1 class="text text--xxlg text--bold">Adicionar bolsa</h1>
        <span class="text" >Filtre e adicione as bolsas de seu interesse</span>
      </div>
      <div class="filters">
        <div class="filters__item">
          <span class="text">Selecione sua cidade</span>
          <select class="filters__item__select" @change="e => onChangeFilter('city', e.target.value)">
            <option value="">Todos</option>
            <option
              v-for="(city, i) in cities"  
              :key="i"
            >
              {{city}}
            </option>
          </select>
        </div>
        <div class="filters__item">
          <span class="text">Selecione o curso de sua preferência</span>
          <select class="filters__item__select" @change="e => onChangeFilter('course', e.target.value)">
            <option value="">Todos</option>
            <option
              v-for="(course, i) in courses"  
              :key="i"
            >
              {{course}}
            </option>
          </select>
        </div>
        <div class="filters__item">
          <span class="text">Como você quer estudar?</span>
          <div class="filters__item__checkbox">
            <div @click="onChangeFilter('kind', filter.kind === 'Presencial' ? '' : 'Presencial')">
              <i :class="`fa${filter.kind === 'Presencial' ? 's' : 'r'} fa-check-square fa-4x`" />
              <span class="text">Presencial</span>
            </div>
            <div @click="onChangeFilter('kind', filter.kind === 'EaD' ? '' : 'EaD')">
              <i :class="`fa${filter.kind === 'EaD' ? 's' : 'r'} fa-check-square fa-4x`" />
              <span class="text">A distância</span>
            </div>
          </div>
        </div>
        <div class="filters__item">
          <span class="text">Até quando você pode pagar?</span>
          <VueSlider
            @change="value => onChangeFilter('price', value)"
            v-model="filter.price"
            :min="Math.min(...prices)"
            :max="Math.max(...prices)"
            :lazy="true"
          />
        </div>
      </div>
      <div class="results">
        <div class="results__infos">
          <span class="text text--bold">Resultado:</span>
          <div class="results__infos--sort">
            <span class="text text--right">
              Ordenar por
            </span>
            <select @change="e => onChangeFilter('sort', e.target.value)">
              <option value="university.name" >Nome da faculdade</option>
              <option value="price_with_discount">Preço</option>
            </select>
          </div>
        </div>
        <div
          v-for="scholarship in sort(filter.sort, scholarships.filter(applyFilter), true)"
          :key="scholarship.id"
          class="results__item"
        >
          <div class="results__item__column results__item__column--check">
            <i
              :class="`fa${selectds.indexOf(scholarship.id) >= 0 ? 's' : 'r'} fa-check-square fa-4x`"
              @click="toggleSelected(scholarship.id)"
            />
          </div>
          <div class="results__item__column results__item__column--img">
            <img class="results__item__column" :src="scholarship.university.logo_url" />
          </div>
          <div class="results__item__column results__item__column--infos">
            <span class="text text--primary text--bold">
              {{scholarship.course.name}}
            </span>
            <span class="text">
              {{scholarship.campus.city}}
            </span>
            <div class="results__item__column results__item__column--prices">
              <span class="text text--right">Bolsa de <span class="text text--green">{{scholarship.discount_percentage}}%</span></span>
              <span class="text text--right text--green text--bold">R${{scholarship.price_with_discount}}</span>
            </div>
          </div>
          <div class="results__item__column results__item__column--prices">
            <span class="text text--right">Bolsa de <span class="text text--green">{{scholarship.discount_percentage}}%</span></span>
            <span class="text text--right text--green text--bold">R${{scholarship.price_with_discount}}</span>
          </div>
        </div>
      </div>
      <div class="buttons">
        <button class="button button--outline" @click="toggleModal()">
          Cancelar
        </button>
        <button
          :class="`button ${selectds.length > 0 ? 'button--yellow' : 'button--disabled' }`"
          @click="selectds.length > 0 && addFavorites()"
        >
          Adicionar bolsa(s)
        </button>
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
  @import "../../../../assets/_text.scss";
  @import "../../../../assets/_button.scss";
  @import "./Modal.scss";
</style>
<script>
import VueSlider from 'vue-slider-component';
import 'vue-slider-component/theme/antd.css';
import { sort } from '../../../../utils';

export default {
  name: 'Modal',
  components: { VueSlider },
  data () {
    return {
      modalState: false,
      sort: sort,
      scholarships: [],
      cities: [],
      courses: [],
      prices: [],
      filter: {
        course: '',
        kind: '',
        city: '',
        price: 0,
        sort: 'university.name'
      },
      selectds: [],
    }
  },
  methods: {
    dataLoaded (favorites, scholarships) {
      this.selectds = favorites.map(item => item.id);
      this.scholarships = sort(this.filter.sort, scholarships, true);
      this.cities = [...new Set(scholarships.map(item => item.campus.city ))];
      this.courses = [...new Set(scholarships.map(item => item.course.name ))];
      this.prices = [...new Set(scholarships.map(item => Math.round(item.price_with_discount) ))];
      this.filter.price = Math.max(...this.prices);
    },
    toggleModal () {
      this.modalState = !this.modalState;
    },
    onChangeFilter (prop, value) {
      this.filter[prop] = value;
    },
    applyFilter ({ price_with_discount, campus, course }) {
      const { filter } = this;
      
      if (filter.kind && course.kind !== filter.kind)
        return false;
      if (price_with_discount > filter.price)
        return false;
      if (filter.city && filter.course)
        return (campus.city === filter.city && course.name === filter.course)
      if (filter.city && campus.city === filter.city)
        return true
      if (filter.course && course.name === filter.course)
        return true
      if (!filter.city && !filter.course)
        return true
      return false;
    },
    toggleSelected (id) {      
      if (this.selectds.indexOf(id) >= 0) {
        this.selectds = this.selectds.filter(item => item !== id);
        return;
      }
      this.selectds = [...this.selectds, id];
    },
    addFavorites () {
      this.$emit('addFavoritesById', this.selectds);
      this.toggleModal();
    }
  }
}
</script>