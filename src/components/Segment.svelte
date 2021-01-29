<script>
  export let value = "";
  export let ref;
</script>

<div
  bind:this={ref}
  id={value}
  class="wheel__segment"
  style="border-top-color: {value || 'red'}"
/>

<style>
  .wheel__segment {
    position: relative;
    --s: 120px;
    --bt: 200px;
    border: 0 solid transparent;
    border-left-width: var(--s);
    border-right-width: var(--s);
    margin-left: calc(var(--s) * -1);
    transform-origin: 50% 100%;
    display: none;
  }
  /* .value {
    position: absolute;
    width: 50px;
    height: 50px;
  } */
  .wheel__segment:nth-child(1) {
    border-top: var(--bt) solid transparent;
    left: 50%;
    position: absolute;
    transform: rotate(0deg);
  }

  .wheel__segment:nth-child(2) {
    left: 50%;
    border-top: var(--bt) solid transparent;
    position: absolute;
    transform: rotate(60deg);
  }

  .wheel__segment:nth-child(3) {
    border-top: var(--bt) solid transparent;

    left: 50%;
    position: absolute;
    transform: rotate(120deg);
  }
  .wheel__segment:nth-child(4) {
    border-top: var(--bt) solid orange;

    left: 50%;
    position: absolute;
    transform: rotate(180deg);
  }
  .wheel__segment:nth-child(5) {
    border-top: var(--bt) solid paleturquoise;

    left: 50%;
    position: absolute;
    transform: rotate(240deg);
  }
  .wheel__segment:nth-child(6) {
    border-top: var(--bt) solid purple;

    left: 50%;
    position: absolute;
    transform: rotate(300deg);
  }
</style>
