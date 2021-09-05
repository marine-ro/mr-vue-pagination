<template>
    <nav aria-label="pagination" :class="paginationClasses.nav" v-if="lastPage > 1">
        <ul :class="paginationClasses.ul">
            <li :class="paginationClasses.li" v-if="showFirstPage">
                <a :class="paginationClasses.link" @click="changePage(firstPage)">
                      <slot name="linkFirst">
                        <span aria-hidden="true">&laquo;</span>
                        <span class="visuallyhidden">first set of pages</span>
                      </slot>
                </a>
            </li>
            <li :class="paginationClasses.li" v-if="internalCurrentPage!==1">
                <a :class="paginationClasses.link" @click="changePage(prevPage)">
                      <slot name="linkPrev">
                        <span aria-hidden="true">&lsaquo;</span>
                        <span class="visuallyhidden">previous set of pages</span>
                      </slot>
                </a>
            </li>

            <li :class="paginationClasses.li" v-if="showMorePageBefore">
                ...
            </li>

            <template v-for="(page) in pagesLeftOfCurrent">
                <li :class="paginationClasses.li" :key="page">
                    <a :class="paginationClasses.link" @click="changePage(page)">{{ page }}</a>
                </li>
            </template>
            <li class="active" :class="paginationClasses.li">
                <a :class="paginationClasses.link" @click="changePage(internalCurrentPage)">
                  {{ internalCurrentPage }}</a>
            </li>

            <template v-for="(page) in pagesRightOfCurrent">
                <li :class="paginationClasses.li" :key="page">
                    <a :class="paginationClasses.link" @click="changePage(page)">{{ page }}</a>
                </li>
            </template>

            <li :class="paginationClasses.link" v-if="showMorePageAfter">
                ...
            </li>

            <li :class="paginationClasses.li" v-if="showLastPage">
                <a :class="paginationClasses.link" @click="changePage(lastPage)" >
                  <slot name="linkLast">
                    <span class="visuallyhidden">page </span>
                    {{ lastPage }}
                  </slot>
                </a>
            </li>

            <li :class="paginationClasses.li" v-if="internalCurrentPage!==lastPage">
                <a :class="paginationClasses.link" @click="changePage(nextPage)">
                      <slot name="linkNext">
                        <span aria-hidden="true">&rsaquo;</span>
                        <span class="visuallyhidden">next set of pages</span>
                      </slot>
                </a>
            </li>
            <li :class="paginationClasses.li" v-if="internalCurrentPage!==lastPage">
                <a :class="paginationClasses.link" @click="changePage(lastPage)">
                      <slot name="linkLast">
                        <span aria-hidden="true">&raquo;</span>
                        <span class="visuallyhidden">last set of pages</span>
                      </slot>
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
import { CLASSES_DEFAULT, LABELS_DEFAULT } from '@/pagination/utils/constants';

export default {
  name: 'Mr-pagination',
  props: {
    externalCurrentPage: {
      type: [Number, String],
      default: 1,
    },
    firstPage: {
      type: Number,
      default: 1,
    },
    lastPage: {
      type: Number,
      default: 1,
    },
    countShowPages: {
      type: Number,
      default: 5,
    },
    perPage: {
      type: Number,
      default: 20,
    },
    classes: {
      type: Object,
      required: false,
      default: () => {},
    },
    labels: {
      type: Object,
      required: false,
      default: () => {},
    },
  },
  data() {
    return {
      paginationClasses: {
        ...CLASSES_DEFAULT,
        ...this.classes,
      },
      paginationLabels: {
        ...LABELS_DEFAULT,
      },
      countExtraPageLeftOfCurrent: 0,
      countExtraPageRightOfCurrent: 0,
    };
  },
  computed: {
    internalCurrentPage() {
      if (this.externalCurrentPage > this.lastPage) {
        return this.lastPage;
      }
      // eslint-disable-next-line max-len
      if (this.externalCurrentPage < 1 || parseInt(this.externalCurrentPage, 10) !== this.externalCurrentPage) {
        return 1;
      }

      return this.externalCurrentPage;
    },

    showMorePageBefore() {
      return (this.internalCurrentPage > 3);
    },

    showMorePageAfter() {
      // eslint-disable-next-line max-len
      return (this.lastPage - (this.internalCurrentPage + this.pagesRightOfCurrent.length) >= this.countShowPagesRightOfCurrent);
    },

    showFirstPage() {
      return this.internalCurrentPage > 3;
    },

    showLastPage() {
      return this.internalCurrentPage + this.pagesRightOfCurrent.length < this.lastPage;
    },

    nextPage() {
      return this.internalCurrentPage + 1;
    },

    prevPage() {
      return this.internalCurrentPage - 1;
    },

    countShowPagesWithoutCurrent() {
      return this.countShowPages - 1;
    },

    countShowPagesLeftOfCurrent() {
      const n = Math.floor(this.countShowPagesWithoutCurrent / 2);
      this.setCountExtraPageLeftOfCurrent(n);

      return n;
    },

    countShowPagesRightOfCurrent() {
      const n = Math.ceil(this.countShowPagesWithoutCurrent / 2);
      this.setCountExtraPageRightOfCurrent(n);
      return n;
    },

    pagesLeftOfCurrent() {
      const pages = [];
      for (let i = this.countShowPagesLeftOfCurrent + this.countExtraPageRightOfCurrent;
        // eslint-disable-next-line no-plusplus
        i > 0; i--) {
        const page = this.internalCurrentPage - i;

        if (page > 0) {
          pages.push(page);
        }
      }

      return pages;
    },

    pagesRightOfCurrent() {
      const pages = [];
      // eslint-disable-next-line no-plusplus
      for (let i = 1;
        i <= this.countShowPagesRightOfCurrent + this.countExtraPageLeftOfCurrent;
        // eslint-disable-next-line no-plusplus
        i++) {
        const page = this.internalCurrentPage + i;

        if (page <= this.lastPage) {
          pages.push(page);
        }
      }

      return pages;
    },

    // computeItemClasses() {
    //   //return `${this.paginationClasses.li} ${hasFirst ? paginationClasses.liDisable : ''}`
    // },
  },
  methods: {
    changePage(page) {
      if (page < 1 || page > this.lastPage) {
        return;
      }

      this.$emit('changePage', page);
    },

    setCountExtraPageLeftOfCurrent(n) {
      if (this.internalCurrentPage - n <= 0) {
        this.countExtraPageLeftOfCurrent = n + 1 - this.internalCurrentPage;
      } else {
        this.countExtraPageLeftOfCurrent = 0;
      }
    },

    setCountExtraPageRightOfCurrent(n) {
      if (this.internalCurrentPage + n > this.lastPage) {
        this.countExtraPageRightOfCurrent = this.internalCurrentPage + n - this.lastPage;
      } else {
        this.countExtraPageRightOfCurrent = 0;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.visuallyhidden {
    border: 0;
    clip: rect(0 0 0 0);
    height: auto;
    margin: 0;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
    white-space: nowrap;
}
nav {
    display: flex;
    justify-content: center;
}

.mr-pagination {
}
.mr-pagination__list {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
}
.mr-pagination__item {
    margin: 0 1px;
    &.active {
      background-color: crimson;
    }
}
.mr-pagination a {
    display: block;
    padding: 0.5em 1em;
    border: 1px solid #999;
    border-radius: 0.2em;
    text-decoration: none;
    cursor: pointer;
}
.mr-pagination a[aria-current="page"] {
    background-color: #333;
    color: #fff;
}
</style>
