<script>
  import Todos from "./Todos.svelte";
  import ApplloClient from "apollo-boost";
  import { setClient } from "svelte-apollo";
  const client = new ApplloClient({
    uri: "https://todos-graphql.herokuapp.com/graphql"
  });
  setClient(client);
</script>

<style>
  main {
    font-family: sans-serif;
    text-align: center;
  }
</style>

<main>
	<h1>Svelte Todos</h1>
	<Todos />
</main>

