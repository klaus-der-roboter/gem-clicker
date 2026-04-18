<script>
  import ClickGame from './ClickGame.svelte';
</script>

<main>
  <ClickGame />
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: #0a0e27;
    font-family: 'Arial', sans-serif;
    color: #fff;
  }

  main {
    width: 100vw;
    height: 100vh;
  }
</style>
