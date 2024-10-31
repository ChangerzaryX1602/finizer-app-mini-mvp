<script>
  import axios from "axios";

    let username = "";
    let password = "";
    let email = "";
    const register = async () => {
      axios.post("http://localhost:5000/register", {
        username,
        password,
        email
      }).then((response) => {
        console.log(response);
        alert("Register successful!");
        window.location.href = "/";
      }).catch((error) => {
        console.error(error);
        alert("Register failed!");
      });
    };
  </script>
  
  <div
    style="display: flex; justify-content: center; align-items: center; height: 100vh;"
  >
    <form style="display: flex; flex-direction: column; align-items: center; width: 400px; padding: 10px; border: 1px solid #ccc; border-radius: 8px;">
      <h1 style="font-size: 2em;">Login</h1>
      <input type="text" placeholder="Username" style="width: 100%; padding: 10px; margin-bottom: 15px; font-size: 1.2em;" bind:value={username}/>
      <input type="password" placeholder="Password" style="width: 100%; padding: 10px; margin-bottom: 15px; font-size: 1.2em;" bind:value={password}/>
      <input type="text" placeholder="Email" style="width: 100%; padding: 10px; margin-bottom: 15px; font-size: 1.2em;" bind:value={email}/>
      <button class="login-button" style="width: 100%; padding: 10px; font-size: 1.2em;" on:click={register}>register</button>
      <a href="/" style="margin-top: 10px;">Have an account? Login here</a>
    </form>
  </div>
  
  <style>
    .login-button {
      background-color: #4caf50; /* Green */
      border: none;
      color: white;
      padding: 15px 32px;
      text-align: center;
      text-decoration: none;
      font-size: 16px;
      margin: 4px 2px;
      cursor: pointer;
    }
  
  </style>
  