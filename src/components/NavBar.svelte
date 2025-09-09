<script>
    let showContact = false;
    let showAboutMe = false;
</script>

<nav>
    <ol>
      <li><button on:click={() => showAboutMe = true}>also me</button></li>
      <li><button on:click={() => showContact = true}>contact</button></li>
    </ol>
</nav>

{#if showAboutMe}
  <div class="modal">
    <div class="modal-content">
      <h2>also me</h2>
      <p>...</p>
      <button on:click={() => showAboutMe = false}>x</button>
    </div>
  </div>
{/if}

{#if showContact}
  <div class="modal">
    <div class="modal-content">
      <h2>contact me</h2>
      <p>...</p>
      <button on:click={() => showContact = false}>x</button>
    </div>
  </div>
{/if}

<style>
    nav {
        position: sticky;
        padding: 1rem;
        width: 100%;
        top: 0;
        z-index: 1;
    }

    ol {
        list-style: none;
        display: flex;
        gap: 1.5rem;
        margin: 0;
        padding: 0;
    }

    li button {
        background: none;
        border: none;
        cursor: pointer;
        font-size: 16px;
        font-weight: 500;
        color: #333;
        transition: color 0.2s ease;
    }

    li button:hover {
        color: #777777;
    }

    .modal {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0, 0, 0, 0.6);
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .modal-content {
        background: #fff;
        padding: 2rem;
        border-radius: 0.75rem;
        max-width: 400px;
        width: 90%;
        text-align: center;
        box-shadow: 0 5px 20px rgba(0,0,0,0.3);
    }

    .modal-content button {
        margin-top: 1rem;
        padding: 0.5rem 1rem;
        background: #0070f3;
        border: none;
        color: white;
        border-radius: 0.5rem;
        cursor: pointer;
    }

    .modal-content button:hover {
        background: #0059c9;
    }
</style>
