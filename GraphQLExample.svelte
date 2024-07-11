<!-- src/routes/GraphQLExample.svelte -->
<script>
    import { client } from '../lib/graphqlClient';
    import { onMount } from 'svelte';
  
    let data = [];
    let loading = false;
    let error = null;
  
    const query = `
      query {
        allUsers {
          id
          name
          email
        }
      }
    `;
  
    async function fetchData() {
      loading = true;
      error = null; // Reset error before fetching
      try {
        const response = await client.request(query);
        data = response.allUsers;
      } catch (err) {
        error = err;
      } finally {
        loading = false;
      }
    }
  </script>
  
  <main>
    <h1>API Fetching</h1>
    <button on:click={fetchData}>Fetch Users</button>
    {#if loading}
      <p>Loading...</p>
    {:else if error}
      <p>Error: {error.message}</p>
    {:else if data.length > 0}
      <ul>
        {#each data as user (user.id)}
          <li>{user.name} ({user.email})</li>
        {/each}
      </ul>
    {:else}
      <p>Click the button to fetch users</p>
    {/if}
  </main>
  
  <style>
    main {
      text-align: center;
      padding: 1em;
      margin: 0 auto;
      max-width: 600px;
      border: 3px solid black;
      border-radius: 15px;
    }
    button {
      margin-bottom: 1em;
      border-radius: 8px;
      background-color: black;
      color: aliceblue;
      width: 150px;
    }
  </style>
  