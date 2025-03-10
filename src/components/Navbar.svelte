<script>
    let token = localStorage.getItem('token');
  
    function logout() {
      localStorage.removeItem('token');
      window.location.href = '/login'; // Átirányítás kijelentkezés után
    }
  </script>
  
  <nav class="navbar">
    <a href="/" class="nav-link">Főoldal</a>
    {#if token}
      <a href="/dashboard" class="nav-link">Dashboard</a>
      <a href="/admin" class="nav-link">Admin</a>
      <button on:click={logout} class="logout-btn">Kilépés</button>
    {:else}
      <a href="/login" class="nav-link">Bejelentkezés</a>
    {/if}
  </nav>
  
  <style>
    .navbar {
      display: flex;
      gap: 10px;
      background: #333;
      padding: 10px;
    }
    .nav-link {
      color: white;
      text-decoration: none;
    }
    .logout-btn {
      background: red;
      color: white;
      border: none;
      cursor: pointer;
    }
  </style>
  