<script>
  import Fortune from "./components/Fortune.svelte";
</script>

<main>
  <Fortune />
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    text-align: center;
    padding: 2em;
    background: url(https://res.cloudinary.com/tumi/image/upload/v1611664122/game-bg.jpg)
      no-repeat;
    background-size: contain;
    background-position: -1% -1%;
    height: 100%;
  }
</style>
