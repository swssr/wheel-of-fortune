<script>
  import qs from "query-string";
  import Select from "svelte-select";

  import { onMount } from "svelte";
  // import { each } from "svelte/internal";
  import Arrow from "./Arrow.svelte";

  // import Segment from "./Segment.svelte";

  export let spinDuration;

  let parsed = {};
  let config = false;
  let gameOdds = [
    { label: "Lose", value: 0 },
    { label: "Win", value: 100 },
  ];
  let currentOdds = { label: "Win", value: 100 };
  let selectedValue = {
    label: "4th_street_yellow",
    value: "4th_street_yellow",
  };
  let future = "";

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
      odds: 100,
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
  const _traps = options.filter((v) => v.id.includes("trap"));
  let _wins = options.filter((v) => !v.id.includes("trap"));

  function computeWin() {
    console.log(_traps);
    const gameOdds = currentOdds.value;
    const _randomNum = Math.random() * 100;

    console.log("odds", gameOdds);
    console.log("random number", _randomNum);

    gameWon = _randomNum < gameOdds;

    if (gameWon) {
      //DONE: Need to randomly select from _wins range ✔.
      //Compute prize with highest winning odds.
      //ALERT: Use dropdown value for testing, revert back to looper on prod.
      // for (const _option of options) {
      //   if (_option.odds >= highestOddsYet) {
      //     optionHighestOds = _option;
      //     highestOddsYet = _option.odds;

      //   }
      optionHighestOds = options.find((v) => v.id === selectedValue.value);
      rotation =
        optionHighestOds.rotation.from +
        Math.floor(1 + Math.random() * increments);
      future = "Should win a " + optionHighestOds.id;
      console.log(future);
    } else {
      //TODO: Need to randomly select from _trap range.
      console.log("Should lose");
      const randomIndex = Math.abs(
        Math.floor(Math.random() * _traps.length - 1)
      );
      rotation =
        _traps[randomIndex].rotation.from +
        Math.floor(1 + Math.random() * increments);
    }
  }
  function spinThatWheel(e) {
    computeWin();
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

  function toggleWin(event) {
    console.log({ event });
    shouldWin = value === "true" ? true : false;
  }
  function handleChange(event) {
    console.log({ event });
    _wins = _wins.map((v) => {
      if (v.id == value) {
        return (v.odds = 100);
      }
    });
  }

  function handleSelect(event) {
    console.log("selected item:", event.detail);
    console.log({ _wins });
    selectedValue = event.detail;
    console.log({ selectedValue });

    _wins = options.map((v) => {
      if (v.id == event.detail.value) {
        future = "Should win a " + event.detail.value;
        return (v.odds = 100);
      }
    });
  }
  function handleWonSelect(event) {
    console.log("selected item:", event.detail);
    currentOdds = event.detail;
    if (event.detail.value === 0) future = "Should lose";
  }

  let items = _wins.map(({ id }) => ({ value: id, label: id }));
  onMount(() => {
    computeWin();

    if (typeof window !== "undefined") {
      parsed = qs.parse(window.location.search);
      console.log({ parsed });
    }
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
    <!-- {#each options as { id, bg } (id)}
      <Segment value={bg} ref={bg} />
    {/each} -->
  </div>

  <button on:click={spinThatWheel} bind:this={resetBtn} class="btn"
    >SPIN WHEEL</button
  >

  <code>Testing: {future}</code>
</div>
<div class="wrapper">
  <button class="btn-config" on:click={() => (config = !config)}
    ><img
      src="https://res.cloudinary.com/tumi/image/upload/v1611929832/settings.svg"
      alt=""
    /></button
  >
  {#if config}
    <div class="test-config">
      <Select
        items={gameOdds}
        selectedValue={currentOdds}
        on:select={handleWonSelect}
        isClearable={false}
      />
      <Select
        isDisabled={!currentOdds.value}
        {items}
        {selectedValue}
        on:select={handleSelect}
        isClearable={false}
      />
    </div>
  {/if}
</div>

<style>
  code {
    position: absolute;
    bottom: 15vh;
    left: 50%;
    transform: translateX(-50%);
    color: #fff;
  }
  .wrapper {
    position: absolute;
    top: 1em;
    right: 1em;
  }
  .btn-config {
    width: 2em;
    height: 2em;
  }
  .test-config {
    position: fixed;
    display: flex;
    height: max-content;
    padding: 1em;
    left: 10%;
    background-color: #fff;
    z-index: 100;
    border-radius: 8px;
    box-shadow: 0 0 26px #000;
  }
  img {
    width: 100%;
    height: 100%;
  }
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
  @media (max-width: 320px) {
    .wheel {
      transform: scale(0.6);
    }
    .arrow {
      transform: scale(0.8);
    }
  }
  .wheel::after {
    content: "";
    position: fixed;
    width: 3.5em;
    height: 3.5em;
    border-radius: 50%;
    background: url(https://res.cloudinary.com/tumi/image/upload/v1611923254/logo.svg)
      no-repeat;
    background-size: cover;
    top: 50%;
    margin-top: -3px;
    transform: translate(-50%, -50%);
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
