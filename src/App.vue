<script>
import { RouterLink, RouterView } from 'vue-router'
import Navbar from "./components/Navbar.vue"
import Footer from "./components/Footer.vue"
export default {
components: {
  RouterLink, 
  RouterView,
  Navbar,
  Footer
}
}
</script>

<template>
  <header>
    <Navbar/>
  </header>

  <RouterView />

  <footer>
    <Footer/>
  </footer>
</template>

<style lang="scss">
@use "./assets/variables.scss" as *;
@use "./assets/mixins.scss" as *;
* {
  margin: 0;
  padding: 0;
  text-decoration: none;
  font-family: 'Roboto', sans-serif;
  font-weight: bold;
  list-style: none;
}

.bg_color {
   background-image: linear-gradient(to bottom, #011d48, #1d1332, #1e0c1f, #160510, #000000);
}

.d_flex {
 display: flex;
 flex-wrap: wrap;
}

.f_column {
  @media(max-width: $mobile_tablet_breakpoint) {
    display: flex;
    flex-direction: column;
  }
}

.text_center {
  text-align: center;
}

.btn {
  background: none;
  border: 2px solid $third_color;
  padding: 1rem;
  border-radius: 1rem;
  color: $third_color;
  margin-top: 2rem;
  cursor:pointer;
  width: 200px;

  &:hover {
    background-color: $third_color;
    color: $primary_color;
    transition: all .75s ease-in-out;
  }
}

.pagination_color {
  text-decoration: none;
  margin: 5px;
  color: #ffff;
  font-size: 18px;
  padding: 0.5rem;
  background-color: $fourth_color;
  border: 1px solid $border_color;
  @include borderRadius;
  width: 80px;
  cursor: pointer;
}

h2 {
  margin-bottom: 2rem;
  font-size: 33px;

  @media(max-width: $mobile_breakpoint) {
    font-size: calc(33px/1.5);
  }
}

h3 {
  @media(max-width: $mobile_breakpoint) {
    font-size: calc(24px/1.5);
  }
}

.secondary_color {
  color: $secondary_color;
}
</style>
