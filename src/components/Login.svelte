<script>
    import { onMount } from "svelte";
    import { API_BASE } from '../config';
    import { navigate } from "svelte-routing";
    import axios from "axios";
    
    let username = "";
    let password = "";
    let errorMessage = "";
    
    async function login() {
        try {
            const response = await axios.post("https://localhost:7032/api/Auth/login", {
                username,
                password
            });
            
            if (response.data.token) {
                localStorage.setItem("token", response.data.token);
                navigate("/dashboard");
            }
        } catch (error) {
            errorMessage = "Hibás felhasználónév vagy jelszó";
        }
    }
    </script>
    
    <main class="login-container">
        <h2>Bejelentkezés</h2>
        <form on:submit|preventDefault={login}>
            <label>Felhasználónév:</label>
            <input type="text" bind:value={username} required />
            
            <label>Jelszó:</label>
            <input type="password" bind:value={password} required />
            
            {#if errorMessage}
                <p class="error">{errorMessage}</p>
            {/if}
    
            <button type="submit">Belépés</button>
        </form>
    </main>
    
    <style>
    .login-container {
        width: 300px;
        margin: 100px auto;
        padding: 20px;
        border: 1px solid #ccc;
        border-radius: 5px;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        text-align: center;
    }
    input {
        width: 100%;
        padding: 8px;
        margin: 8px 0;
        display: block;
    }
    .error {
        color: red;
    }
    </style>
    