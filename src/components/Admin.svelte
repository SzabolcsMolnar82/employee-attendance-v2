<script>
    import { onMount } from 'svelte';
    import { API_BASE } from '../config';
  
    let token = localStorage.getItem('token');
    let employees = [];
    let newEmployee = { nev: '', felhasznaloNev: '', jelszo: '' };
  
    if (!token) {
      window.location.href = '/login';
    }
  
    async function fetchEmployees() {
      const res = await fetch(`${API_BASE}/Admin/get-employees`, {
        method: 'GET',
        headers: { 'Authorization': `Bearer ${token}` }
      });
  
      if (res.ok) {
        employees = await res.json();
      } else {
        console.error('Hiba a dolgozók lekérésekor');
      }
    }
  
    async function addEmployee() {
      const res = await fetch(`${API_BASE}/Admin/add-employee`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${token}`
        },
        body: JSON.stringify(newEmployee)
      });
  
      if (res.ok) {
        newEmployee = { nev: '', felhasznaloNev: '', jelszo: '' };
        fetchEmployees();
      } else {
        console.error('Hiba a dolgozó hozzáadásakor');
      }
    }
  
    async function deleteEmployee(id) {
      const res = await fetch(`${API_BASE}/Admin/delete-employee/${id}`, {
        method: 'DELETE',
        headers: { 'Authorization': `Bearer ${token}` }
      });
  
      if (res.ok) {
        fetchEmployees();
      } else {
        console.error('Hiba a dolgozó törlésekor');
      }
    }
  
    function logout() {
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
  
    onMount(fetchEmployees);
  </script>
  
  <main>
    <h1>Admin Felület</h1>
    <button on:click={logout}>Kilépés</button>
  
    <h2>Dolgozók</h2>
    <ul>
      {#each employees as employee}
        <li>
          {employee.nev} - {employee.felhasznaloNev}
          <button on:click={() => deleteEmployee(employee.id)}>Törlés</button>
        </li>
      {/each}
    </ul>
  
    <h2>Új dolgozó hozzáadása</h2>
    <input bind:value={newEmployee.nev} placeholder="Név" />
    <input bind:value={newEmployee.felhasznaloNev} placeholder="Felhasználónév" />
    <input bind:value={newEmployee.jelszo} type="password" placeholder="Jelszó" />
    <button on:click={addEmployee}>Hozzáadás</button>
  </main>