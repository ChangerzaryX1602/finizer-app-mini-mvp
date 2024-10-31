<script>
  import axios from "axios";
  import { onMount } from "svelte";
  const login = () => {
    window.location.href =
      "https://access.line.me/oauth2/v2.1/authorize?response_type=code&client_id=2006502412&redirect_uri=https://finizer-app-mini-mvp.vercel.app/login&state=random&scope=profile%20openid%20email&nonce=finizer";
  };
  function extractCodeFromURL() {
        const currentURL = window.location.href;
        const urlParams = new URLSearchParams(currentURL.split('?')[1]);
        const code = urlParams.get('code');
        return code;
    }
    async function handleAuthentication() {
        const code = extractCodeFromURL();
        if(localStorage.getItem("token")){
            window.location.href = "/cat";
        }
        if (code) {
            try {
                const response = await axios.post("https://app.finizer.co/api/v1/auth/", {
                    code: code
                });
                localStorage.setItem("token", response.data.token);
                window.location.href = "/cat";
            } catch (error) {
                console.error('Error during authentication:', error);
                alert("Login failed!");
            }
        }
      }
    onMount(()=>{handleAuthentication()});
</script>

<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css"
/>
<div class="page-wrapper">
  <div class="page-body">
    <div class="container-xl d-flex flex-column justify-content-center">
      <div
        class="empty d-flex flex-column align-items-center justify-content-center"
      >
        <div class="empty-img">
          <img
            src="./static/illustrations/undraw_printing_invoices_5r4r.svg"
            height="128"
            alt=""
          />
        </div>
        <p class="empty-title" style="font-size: 2.5rem;">ยังไม่ได้ล็อคอิน</p>
        <p class="empty-subtitle text-secondary" style="font-size:1.5rem;">
          กรุณาล็อคอินใหม่อีกครั้ง.
        </p>
        <div class="empty-action">
          <button on:click={login} class="btn btn-primary">
            <i class="bi bi-box-arrow-in-right" style="font-size: 2.5rem;"></i> Login
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
