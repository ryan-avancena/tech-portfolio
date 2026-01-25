<script>
    export let bg = "#fff";
  </script>
  
  <section style="background:{bg}">
    <slot />
  </section>
  
  <style>
    section {
      min-width: 100vw;
      height: 100vh;
      padding: 2rem;
      box-sizing: border-box;
      overflow-y: auto;
    }
  </style>
  