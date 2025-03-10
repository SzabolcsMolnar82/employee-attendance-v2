<script>
    import { onMount } from 'svelte';
    import { API_BASE } from '../config';
  
    let token = localStorage.getItem('token');
    let attendance = [];
  
    if (!token) {
      window.location.href = '/login';
    }
  
    async function fetchAttendance() {
      const res = await fetch(`${API_BASE}/Attendance/get-attendance`, {
        method: 'GET',
        headers: { 'Authorization': `Bearer ${token}` }
      });
  
      if (res.ok) {
        attendance = await res.json();
      } else {
        console.error('Hiba a belépési adatok lekérésekor');
      }
    }
  
    async function checkIn() {
      await fetch(`${API_BASE}/Attendance/check-in`, {
        method: 'POST',
        headers: { 'Authorization': `Bearer ${token}` }
      });
      fetchAttendance();
    }
  
    async function checkOut() {
      await fetch(`${API_BASE}/Attendance/check-out`, {
        method: 'POST',
        headers: { 'Authorization': `Bearer ${token}` }
      });
      fetchAttendance();
    }
  
    function logout() {
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
  
    onMount(fetchAttendance);
  </script>
  
  <main>
    <h1>Dolgozói Dashboard</h1>
    <button on:click={logout}>Kilépés</button>
  
    <button on:click={checkIn}>Munka megkezdése</button>
    <button on:click={checkOut}>Munka befejezése</button>
  
    <h2>Napi jelenlét</h2>
    <ul>
      {#each attendance as entry}
        <li>{entry.timestamp} - {entry.status}</li>
      {/each}
    </ul>
  </main>