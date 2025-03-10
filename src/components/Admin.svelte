<script>
    let token = localStorage.getItem('token');
    let employees = []; // Itt deklaráljuk a változót
  
    if (!token) {
      window.location.href = '/login'; // Irányítás bejelentkezési oldalra
    }
  
    async function fetchEmployees() {
      const res = await fetch('http://localhost:7032/api/Employees', {
        method: 'GET',
        headers: { 'Authorization': `Bearer ${token}` }
      });
  
      if (res.ok) {
        employees = await res.json();
      } else {
        console.error('Hiba a dolgozók lekérésekor');
      }
    }
  
    function logout() {
      localStorage.removeItem('token');
      window.location.href = '/login'; // Kijelentkezés után átdob a login oldalra
    }
  </script>
  
  <main class="container">
    <h1>Admin felület</h1>
    <button on:click={logout}>Kilépés</button>
    <h2>Dolgozók listája</h2>
    <ul>
      {#each employees as employee}
        <li>{employee.name} - {employee.position}</li>
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