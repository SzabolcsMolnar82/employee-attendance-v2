<script>
    import { onMount } from 'svelte';
  
    let token = localStorage.getItem('token');
    let attendance = [];
  
    if (!token) {
      window.location.href = '/login'; // Ha nincs token, irány a login oldal
    }
  
    async function fetchAttendance() {
      const res = await fetch('http://localhost:7032/api/Attendance/check-in', {
        method: 'GET',
        headers: { 'Authorization': `Bearer ${token}` }
      });
  
      if (res.ok) {
        attendance = await res.json();
      } else {
        console.error('Hiba a belépési adatok lekérésekor');
      }
    }
  
    function logout() {
      localStorage.removeItem('token');
      window.location.href = '/login'; // Kijelentkezés után visszadob a bejelentkezési oldalra
    }
  
    onMount(fetchAttendance);
  </script>
  
  <main class="container">
    <h1>Dolgozói Dashboard</h1>
    <button on:click={logout}>Kilépés</button>
    <h2>Napi jelenlét</h2>
    <ul>
      {#each attendance as entry}
        <li>{entry.timestamp} - {entry.status}</li>
      {/each}
    </ul>
  </main>
  
  <style>
    .container {
      max-width: 600px;
      margin: auto;
      text-align: center;
    }
  </style>
  