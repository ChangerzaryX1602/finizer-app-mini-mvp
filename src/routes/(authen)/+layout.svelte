<script>
  import { onMount } from "svelte";
  import { goto } from '$app/navigation'
  import { jwtDecode } from 'jwt-decode';
  function isTokenValid(token) {
    try {
        const decodedToken = jwtDecode(token);
        const currentTime = Math.floor(Date.now() / 1000);
        if (decodedToken.exp && decodedToken.exp < currentTime) {
            console.log("Token has expired.");
            return false;
        }
        console.log("Token is valid.");
        return true;
    } catch (error) {
        console.error("Invalid token format.");
        return false;
    }
}
  onMount(() => {

  });
</script>

<slot />