<template>
  <div class="page-pagination">
    <MrPagination
      :externalCurrentPage="pagination.externalCurrentPage"
      :lastPage="pagination.lastPage"
      @changePage="changePage"
    />
  </div>
</template>

<script>
import MrPagination from './pagination/components/MrPagination.vue';

export default {
  name: 'App',
  components: {
    MrPagination,
  },
  data() {
    return {
      pagination: {
        externalCurrentPage: 1,
        lastPage: 10,
        total: 0,
      },
    };
  },

  methods: {
    changePage(page) {
      this.pagination.externalCurrentPage = page;
    },
  },
};
</script>
