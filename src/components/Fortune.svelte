<script>
  import { onMount } from "svelte";
  import { each } from "svelte/internal";
  import Segment from "./Segment.svelte";

  let wheel;
  let resetBtn;

  let rotation = 0;
  let scale = 1;
  let slowingDown = false;
  let pressed = {};

  window.addEventListener("keydown", handleKeyDown, false);
  window.addEventListener("keyup", handleKeyUp, false);

  function handleKeyDown(e) {
    if (e.keyCode === 32 && slowingDown === false) {
      animateKeyOut();
      if (pressed[e.which]) return;
      pressed[e.which] = e.timeStamp;
    }
  }

  function handleKeyUp(e) {
    if (e.keyCode === 32) {
      animateKeyIn();
      spinThatWheel(e);
    }
  }

  function animateKeyOut() {
    scale -= 0.03;
    if (scale < 0.5) return;
    TweenMax.to(resetBtn, 0.5, { ease: Sine.easeOut, scale: scale });
  }

  function animateKeyIn() {
    scale = 1;
    TweenMax.to(resetBtn, 0.25, { scale: scale });
  }

  function spinThatWheel(e) {
    if (!pressed[e.which]) return;
    let duration = Math.floor((e.timeStamp - pressed[e.which]) / 25);

    if (duration > 150) duration = 150;

    animateWheel(duration);
    slowingDown = true;
    pressed[e.which] = 0;
  }

  function animateWheel(duration) {
    let randomNum = Math.floor(Math.random() * 10);
    rotation += 30 * duration + 30 * randomNum;
    TweenMax.to(wheel, 20, {
      ease: Power4.easeOut,
      rotation: rotation,
      onComplete: resetWheel,
    });
  }

  function resetWheel() {
    slowingDown = false;
  }

  onMount(() => {
    console.log({ wheel, resetBtn });
  });
</script>

<div class="game">
  <header class="logo">
    <strong>BRAND NAME</strong>
    <h1>WHEEL OF FORTUNE</h1>
    <p>TAP <strong>SPIN WHEEL</strong> TO PLAY</p>
  </header>

  <div class="arrow" />

  <div bind:this={wheel} class="wheel">
    {#each [1, 2, 3, 4, 5, 6] as value}
      <Segment {value} />
    {/each}
  </div>

  <button bind:this={resetBtn} class="btn">SPIN WHEEL</button>
</div>

<style>
  .game {
    display: flex;
    overflow: hidden;
  }
  .logo {
    width: 100%;
    height: min-content;
    margin-top: 2em;
    color: #fff;
  }
  h1 {
    font-size: 28px;
    color: var(--color-gold);
  }
  .arrow {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    margin-top: -205px;
    border-left-width: 10px;
    border-right-width: 10px;
    border-top: 30px solid white;
    z-index: 3;
  }

  .wheel {
    background-color: white;
    width: 400px;
    height: 400px;
    margin-left: -200px;
    margin-top: -200px;
    border-radius: 50%;
    position: absolute;
    top: 55%;
    left: 50%;
    border: 2px solid white;
    overflow: hidden;
    box-shadow: 0px 0px 10px 10px #000;
    transform: scale(0.75);
  }

  .btn {
    padding: 10px 50px;
    border: 0;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 30px;
    color: black;
    font-weight: 700;
    text-transform: uppercase;
    background: #fff;
    width: 80%;
  }
  .reset-btn:focus {
    outline: none;
  }
  .reset-btn:disabled {
    box-shadow: none;
    opacity: 0.5;
    color: grey;
  }
  .reset-btn:active {
    box-shadow: none;
  }
</style>
