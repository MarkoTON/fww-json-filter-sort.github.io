<template>
  <div v-if="totalPages() > 0" class="pagination-wrapper text-center">
    <span v-if="showPreviousLink()" class="pagination-btn" v-on:click="updatePage(currentPage - 1)"> &lsaquo; </span>
    {{ currentPage + 1 }} of {{ totalPages() }}
    <span v-if="showNextLink()" class="pagination-btn" v-on:click="updatePage(currentPage + 1)"> 	&rsaquo; </span>
  </div>
</template>

<script>
export default {
  name: 'pagination',
  props: ['stateUsers', 'currentPage', 'pageSize'],
  methods: {
    updatePage(pageNumber) {
      this.$emit('page-update', pageNumber);
    },
    totalPages() {
      console.log(this.stateUsers);
      console.log(this.stateUsers.length);
      console.log(Math.ceil(this.stateUsers.length / this.pageSize));
      return Math.ceil(this.stateUsers.length / this.pageSize);
    },
    showPreviousLink() {
      return this.currentPage == 0 ? false : true;
    },
    showNextLink() {
      return this.currentPage == (this.totalPages() - 1) ? false : true;
    }
  },
  created(){
    // console.log(this.currentPage)
    // console.log(this.pageSize)
    // console.log(this.updatePage(2))
    console.log(this.totalPages());
  }
}

</script>
<style>
.pagination-btn {
  cursor: pointer;
}
</style>
