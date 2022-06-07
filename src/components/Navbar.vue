<script>
import dataJson from "../data/data.json";
export default {
  data() {
    return {
      dataTitle: dataJson.titleComponent,
      datas: dataJson.navbarComponent
    }
  }
}
</script>


<template>
  <div class="navbar d_flex f_column">
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
@include navbarStyle;
</style>
