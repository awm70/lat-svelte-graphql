<script>
  import { getClient, query, mutate } from "svelte-apollo";
  import { gql } from "apollo-boost";

  const GET_TODOS = gql`
    {
      getTodos {
        id
        text
        done
      }
    }
  `;

  const ADD_TODO = gql`
    mutation($text: String!) {
      addTodo(text: $text) {
        id
        text
        done
      }
    }
  `;
  let text = "";
  const client = getClient();
  const todos = query(client, { query: GET_TODOS });

  function addTodo() {
    mutate(client, {
      mutation: ADD_TODO,
      variables: { text }
    })
      .then(() => {
        text = "";
        todos.refetch();
      })
      .catch(err => {
        console.log(err);
      });
  }
</script>

<style>
  ul {
    list-style: none;
  }

  .done {
    text-decoration: line-through;
  }

  .error {
    color: red;
  }
</style>

<h2>Todos</h2>
<form on:submit|preventDefault={addTodo}>
  <input bind:value={text} placeholder="Add your todo">
  <button>Submit</button>
</form>

{#await $todos}
  Loading...
{:then result}
  <ul>
  {#each result.data.getTodos as todo}
  <li class:done="{todo.done}">
  {todo.text}
  </li>
  {/each}
  </ul>
{:catch error}
<p class='error'>{error}</p>
{/await}