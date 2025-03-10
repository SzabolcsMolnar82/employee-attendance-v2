<script>
  import Router from 'svelte-spa-router';
  import Navbar from './components/Navbar.svelte';
  import Dashboard from './components/Dashboard.svelte';
  import Admin from './components/Admin.svelte';
  import Login from './components/Login.svelte';

  let routes = {
    '/': Dashboard,
    '/admin': Admin,
    '/login': Login
  };
</script>

<Navbar />

<main>
  <Router {routes} />
</main>

<style>
  main {
    padding: 20px;
  }
</style>