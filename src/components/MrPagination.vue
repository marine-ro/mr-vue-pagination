<template>
    <nav aria-label="pagination" class="mr-pagination">
        <ul class="mr-pagination__list">
            <li class="mr-pagination__item">
                <a href="" class="mr-pagination__link">
                  <span aria-hidden="true">«</span>
                  <span class="visuallyhidden">previous set of pages</span>
                </a>
            </li>
            <li class="mr-pagination__item">
                <a href="" class="mr-pagination__link">
                  <span class="visuallyhidden">page </span>
                  1
                </a>
            </li>
            <li class="mr-pagination__item">
                <a href="" class="mr-pagination__link"><span class="visuallyhidden">next set of pages</span>
                  <span aria-hidden="true">»</span>
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
  name: 'Mr-pagination',
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
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
.mr-pagination li {
    margin: 0 1px;
}
.mr-pagination a {
    display: block;
    padding: 0.5em 1em;
    border: 1px solid #999;
    border-radius: 0.2em;
    text-decoration: none;
}
.mr-pagination a[aria-current="page"] {
    background-color: #333;
    color: #fff;
}
</style>
