<script>
import dataJson from "../data/data.json";
export default {
  data() {
    return {
      dataTitle: dataJson.titleComponent,
      datas: dataJson.navbarComponent,
      view: {
        topOfPage: true
      }
    }
  },
  beforeMount() {
    window.addEventListener('scroll', this.handleScroll);
  },
  methods: {
    handleScroll(){
      let scroll = (window.pageYOffset>50) ? this.view.topOfPage = false : this.view.topOfPage = true
      return scroll;
    }
  }
}
</script>


<template>
  <div class="navbar d_flex f_column" :class="{ 'onScroll': !view.topOfPage}">
    <div class="title d_flex" v-for="data in dataTitle" :key="data.id">
      <img :src="'/' + data.img" :alt="data.name"/>
      <h1>{{data.title}}</h1>
    </div>
    <nav v-for="data in datas" :key="data.id">
      <RouterLink active-class="active-link" exact-active-class="exact-active-link" id="link" :to="data.link">{{data.name}}</RouterLink>
    </nav>
  </div>
</template>


<style scoped lang="scss">
@use "../assets/mixins.scss" as *;
@include onScrollStyle;
@include navbarStyle;
</style>
