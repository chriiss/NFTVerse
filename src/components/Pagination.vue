<script>
export default {
  name: 'pagination',
  props: {
    maxVisibleButtons: {
      type: Number,
      required: false,
      default: 3
    },
    totalPages: {
      type: Number,
      required: true
    },
    perPage: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  computed: {
    isInFirstPage () {
      return this.currentPage === 1
    },
    isInLastPage () {
      if (this.totalPages === 0) {
        return true
      }
      return this.currentPage === this.totalPages
    },
    startPage () {
      // When on the first page
      if (this.currentPage === 1) {
        return 1
      }
      // When on the last page
      if (this.totalPages < this.maxVisibleButtons) {
        return 1
      }
      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons + 1
      }
      // When in between
      return this.currentPage - 1
    },
    endPage () {
      if (this.totalPages === 0) {
        return 1
      }
      if (this.totalPages < this.maxVisibleButtons) {
        return this.totalPages
      }
      return Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages)
    },
    pages () {
      const range = []
      for (let i = this.startPage; i <= this.endPage; i++) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage
        })
      }
      return range
    }
  },
  methods: {
    onClickFirstPage () {
      if (this.isInFirstPage) {
        return false
      }
      this.$emit('pagechanged', 1)
    },
    onClickPreviousPage () {
      if (this.isInFirstPage) {
        return false
      }
      this.$emit('pagechanged', this.currentPage - 1)
    },
    onClickPage (page) {
      this.$emit('pagechanged', page)
    },
    onClickNextPage () {
      if (this.isInLastPage) {
        return false
      }
      this.$emit('pagechanged', this.currentPage + 1)
    },
    onClickLastPage () {
      console.log('onClickLastPage')
      if (this.isInLastPage) {
        return false
      }
      this.$emit('pagechanged', this.totalPages)
    },
    isPageActive (page) {
      return this.currentPage === page
    }
  }
}
</script>
<template>
  <ul class="pagination">
    <li class="pagination-item">
      <button class="pagination_color" @click.prevent="onClickFirstPage" :class="isInFirstPage? 'disabled':''" :disabled="isInFirstPage">First</button>
    </li>
    <li class="pagination-item">
      <button class="pagination_color" @click.prevent="onClickPreviousPage" :class="isInFirstPage? 'disabled':''" :disabled="isInFirstPage">&#x3c;</button>
    </li>
    <li :key="page.id" v-for="page in pages" class="pagination-item">
      <button class="pagination_color" @click.prevent="onClickPage(page.name)" :disabled="page.isDisabled"
        :class="{ active: isPageActive(page.name) }">{{ page.name }}</button>
    </li>
    <li class="pagination-item">
      <button class="pagination_color" @click.prevent="onClickNextPage" :class="isInLastPage? 'disabled':''" :disabled="isInLastPage">&#x3e;</button>
    </li>
    <!--<li class="pagination-item">
      <button @click.prevent="onClickLastPage" :class="isInLastPage? 'disabled':''" :disabled="isInLastPage">Last</button>
    </li>-->
  </ul>
</template>


<style scoped lang="scss">
@use "../assets/mixins.scss" as *;
@include paginationStyle;

</style>