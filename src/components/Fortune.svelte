<script>
  import { onMount } from "svelte";
  import { each } from "svelte/internal";
  import Arrow from "./Arrow.svelte";

  import Segment from "./Segment.svelte";

  export let spinDuration;

  let wheel;
  let resetBtn;
  let refs = {};

  let rotation = 3780;
  let scale = 1;
  let slowingDown = false;
  let pressed = {};

  let optionHighestOds;
  let highestOddsYet = 0;

  let gameWon;

  //45deg increments from first prize
  //Basically brute forced the rotation
  const increments = 43; // Will be usefull later on.
  const options = [
    { id: "trap_1", odds: 90, bg: "", rotation: { from: 3498, to: 3539 } },
    {
      id: "savanna_pack_bottle",
      odds: 60,
      bg: "",
      rotation: { from: 3539, to: 3585 },
    },
    {
      id: "4th_street_yellow",
      odds: 100,
      bg: "",
      rotation: { from: 3585, to: 3630 },
    },
    { id: "trap_2", odds: 0, bg: "", rotation: { from: 3630, to: 3675 } },
    { id: "paarl", odds: 10, bg: "", rotation: { from: 3675, to: 3720 } },
    {
      id: "hunters_pack",
      odds: 40,
      bg: "",
      rotation: { from: 3720, to: 3765 },
    },
    { id: "trap_3", odds: 0, bg: "", rotation: { from: 3765, to: 3810 } },
  ];

  function computeWin() {
    const _traps = options.filter((v) => v.id.includes("trap"));
    const _wins = options.filter((v) => !v.id.includes("trap"));

    console.log(_traps);
    const gameOdds = 100;
    const _randomNum = Math.random() * 100;

    console.log("odds", gameOdds);
    console.log("random number", _randomNum);

    gameWon = _randomNum < gameOdds;

    if (gameWon) {
      //DONE: Need to randomly select from _wins range ✔.
      //Compute prize with highest winning odds.
      console.log({ _wins });
      for (const _option of _wins) {
        if (_option.odds >= highestOddsYet) {
          optionHighestOds = _option;
          highestOddsYet = _option.odds;

          rotation =
            optionHighestOds.rotation.from +
            Math.floor(Math.random() * increments);
        }
      }
      console.log("Should win a", optionHighestOds.id);
    } else {
      //TODO: Need to randomly select from _trap range.
      console.log("Should lose");
      const randomIndex = Math.abs(
        Math.floor(Math.random() * _traps.length - 1)
      );
      rotation =
        _traps[randomIndex].rotation.from +
        Math.floor(Math.random() * increments);
    }
  }
  function spinThatWheel(e) {
    animateWheel(spinDuration);
    slowingDown = true;
  }

  function animateWheel(duration) {
    TweenMax.to(wheel, 10, {
      ease: Power4.easeOut,
      rotation,
      onComplete,
    });
  }

  function onComplete() {
    console.log({ gameWon });
    slowingDown = false;
    alert(
      gameWon
        ? `Awesome! You won. a ${optionHighestOds.id}`
        : "Sorry, you lost."
    );
    //Need a blocking function before this executes
    TweenMax.to(wheel, 1, {
      ease: Power4.easeOut,
      rotation: 0,
    });
  }

  onMount(() => {
    computeWin();
  });
</script>

<div class="game">
  <header class="logo">
    <strong>BRAND NAME</strong>
    <h1>WHEEL OF FORTUNE</h1>
    <p>TAP <strong>SPIN WHEEL</strong> TO PLAY</p>
  </header>
  <Arrow />
  <div bind:this={wheel} class="wheel">
    {#each options as { id, bg } (id)}
      <Segment value={bg} ref={bg} />
    {/each}
  </div>

  <button on:click={spinThatWheel} bind:this={resetBtn} class="btn"
    >SPIN WHEEL</button
  >
</div>

<style>
  .game {
    display: flex;
    overflow: hidden;
  }
  .logo {
    width: 100%;
    height: min-content;
    color: #fff;
  }
  h1 {
    font-size: 28px;
    color: var(--color-gold);
  }

  .wheel {
    /* background-color: white; */
    background-image: url("https://res.cloudinary.com/tumi/image/upload/v1611906746/wheel2.png");
    background-size: cover;
    width: 400px;
    height: 400px;
    margin-left: -200px;
    margin-top: -200px;
    border-radius: 50%;
    position: absolute;
    top: 55%;
    left: 50%;
    overflow: hidden;
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
</style>
