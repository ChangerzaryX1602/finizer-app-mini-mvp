<script>
  import { onMount } from "svelte";
  let images = [];
  let selectedImage = null;
  let isZoom = false;
  async function fetchImages() {
    try {
      images = [];
      const response = await fetch(
        "https://api.thecatapi.com/v1/images/search?limit=10"
      );
      images = await response.json();
    } catch (error) {
      console.error("Error fetching images:", error);
    }
  }

  function handleButtonClick(image) {
    selectedImage = image;
    isZoom = true;
  }

  onMount(() => {
    fetchImages();
  });
</script>

<main>
  <div>
    {#if !isZoom}
      <button
        on:click={fetchImages}
        placeholder="สุ่มรูปใหม่"
        style="width: 100%; padding: 10px; font-size: 1.2em;"
        class="bg-green-500">สุ่มรูปใหม่</button
      >
      {#each images as image, index}
        <h1>รูปที่ {index + 1}</h1>
        <button on:click={handleButtonClick(image)} class="button">
          {#if images.length > 0}
            <img src={image.url} alt="cat" class="w-full h-full object-cover" />
          {:else}
            <p>Loading...</p>
          {/if}
        </button>
      {/each}
    {:else}
      <button on:click={() => (isZoom = false)}>
        <img
          src={selectedImage.url}
          alt="cat"
          class="w-full h-full object-cover"
        />
      </button>
    {/if}
    <br />
    <a
      href="/cat"
      style="width: 100%; padding: 10px; font-size: 1.2em;"
      class="random-button">สุ่มทีละรูป</a
    >
  </div>
</main>

<style>
  .button {
    width: 200px;
    height: 200px;
    margin: 10px;
    border: 1px solid #ccc;
    border-radius: 8px;
    overflow: hidden;
  }
  .random-button {
    background-color: #4caf50; /* Green */
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    display: flex;
        justify-content: center;
        align-items: center;
  }
</style>
