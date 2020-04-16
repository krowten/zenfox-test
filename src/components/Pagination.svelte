<script>
  import { goto, stores } from "@sapper/app";

  const { page: router } = stores();

  export let perPage = 5; //Default 5
  export let totalCount = 0;
  let currentPage = 1;
  let pagesCount = Math.ceil(totalCount / perPage);

  $: currentPage = parseInt($router.query.page || 1);

  const setPage = async page => {
    if (page >= 1 && page <= pagesCount) {
      const queryParams = new URLSearchParams({
        ...$router.query,
        page
      }).toString();
      goto(`${$router.path}?${queryParams}`);
    }
  };
</script>

<ul class="pagination">
  <li class="page-item {currentPage <= 1 ? 'disabled' : ''}">
    <button class="page-link" on:click={e => setPage(1)}>&lt;&lt; First</button>
  </li>
  <li class="page-item {currentPage <= 1 ? 'disabled' : ''}">
    <button class="page-link" on:click={e => setPage(currentPage - 1)}>
      &lt; Previous
    </button>
  </li>
  <li class="page-item disabled">
    <button class="page-link">{currentPage}</button>
  </li>
  <li class="page-item {currentPage >= pagesCount ? 'disabled' : ''}">
    <button class="page-link" on:click={e => setPage(currentPage + 1)}>
      Next &gt;
    </button>
  </li>
  <li class="page-item {currentPage >= pagesCount ? 'disabled' : ''}">
    <button class="page-link" on:click={e => setPage(pagesCount)}>
      Last &gt;&gt;
    </button>
  </li>
</ul>
