<script>
    import axios from "axios";
    import { onMount } from "svelte";
    let ImageURL = "";
    const requestAPI = async () => {
      try {
        ImageURL = "";
        const response = await axios.get(
          "https://api.thecatapi.com/v1/images/search"
        );
        if (response.data && response.data.length > 0) {
          ImageURL = response.data[0].url;
        }
      } catch (error) {
        console.error(error);
      }
    };
    onMount(() => {
      requestAPI();
    });
  </script>
  
  <button
    class="w-full h-full"
    tabindex="0"
    aria-label="Get a random cat image"
    on:click={requestAPI}
  >
    {#if ImageURL}
      <img class="w-full h-full" src={ImageURL} alt="random img" />
    {:else}
      <p>Loading...</p>
    {/if}
  </button>
  <br />
    <a
      href="/cat/cat_selected"
      style="width: 100%; padding: 10px; font-size: 1.2em;"
      class="random-button">สุ่มทีละ 10 รูป</a
    >
  <style lang="postcss">
    :global(html) {
      background-color: theme(colors.gray.100);
    }
    .cat {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100vw; /* Full viewport width */
      height: 100vh; /* Full viewport height */
    }
    .random-button {
        display: flex;
        justify-content: center;
        align-items: center;
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
  