<template>
  <div class="page-pagination">
    <MrPagination />
  </div>
</template>

<script>
import MrPagination from './components/MrPagination.vue';

export default {
  name: 'App',
  components: {
    MrPagination,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
