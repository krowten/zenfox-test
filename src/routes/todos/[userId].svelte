<script context="module">
  import config from "../../config";
  export async function preload({ params }) {
    const { userId } = params;
    const todosRes = await this.fetch(
      `${config.baseUrl}/todos?userId=${userId}`
    );
    const userRes = await this.fetch(
      `${config.baseUrl}/users?userId=${userId}`
    );
    const todos = await todosRes.json();
    const userInfo = await userRes.json();
    return {
      todos,
      userInfo: userInfo && userInfo.find(user => user.id == userId)
    };
  }
</script>

<script>
  import { goto } from "@sapper/app";

  export let todos;
  export let userInfo;

  const back = e => {
    if (history.length > 2) {
      history.back();
    } else {
      goto("/users");
    }
  };
</script>

<style collapsed>
  .top-nav {
    display: flex;
    padding: 20px;
  }
  .title {
    margin: 0 15px;
    font-size: 1.7rem;
  }
</style>

<div class="top-nav">
  <button class="btn btn-outline-secondary" on:click={back}>&lt; Back</button>
  <div class="title">{userInfo.name}</div>
</div>

<ul class="list-group">
  {#each todos as todo}
    <li class="list-group-item">
      <span class="badge badge-{todo.completed ? 'success' : 'warning'}">
        {todo.completed ? 'Completed' : 'In progress'}
      </span>
      {todo.title}
    </li>
  {/each}
</ul>
