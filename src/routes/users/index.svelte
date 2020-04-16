<script context="module">
  import config from "../../config";
  export async function preload({ query }) {
    const { page } = query;
    const res = await this.fetch(
      `${config.baseUrl}/users?_page=${page}&_limit=5`
    );
    const totalCount = res.headers.get("x-total-count");
    const users = await res.json();
    if (res.status === 200) {
      return { users, totalCount };
    } else {
      this.error(res.status);
    }
  }
</script>

<script>
  import Pagination from "../../components/Pagination.svelte";

  export let users;
  export let totalCount;
</script>

<table class="table">
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Email</th>
      <th scope="col">Phone</th>
    </tr>
  </thead>
  <tbody>
    {#each users as user}
      <tr>
        <td>
          <a href="todos/{user.id}">{user.name}</a>
        </td>
        <td>
          <a href="mailto:{user.email}">{user.email}</a>
        </td>
        <td>{user.phone}</td>
      </tr>
    {/each}
  </tbody>
</table>
<Pagination perPage={5} {totalCount} />
