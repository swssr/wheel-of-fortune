<script>
  import { onMount } from "svelte";
  import { each } from "svelte/internal";

  import Segment from "./Segment.svelte";

  export let spinDuration;

  let wheel;
  let resetBtn;
  let refs = {};

  let rotation = 3780;
  let scale = 1;
  let slowingDown = false;
  let pressed = {};

  window.addEventListener("keydown", handleKeyDown, false);
  window.addEventListener("keyup", handleKeyUp, false);

  //40deg increments from first prize
  const options = [
    { id: "trap_1", bg: "", rotation: { from: 3498, to: 3539 } },
    { id: "savanna_pack_bottle", bg: "", rotation: { from: 3539, to: 3585 } },
    { id: "4th_yellow", bg: "", rotation: { from: 3585, to: 3630 } },
    { id: "trap_2", bg: "", rotation: { from: 3630, to: 3675 } },
    { id: "paarl", bg: "", rotation: { from: 3675, to: 3720 } },
    { id: "hunters_pack", bg: "", rotation: { from: 3720, to: 3765 } },
    { id: "trap_3", bg: "", rotation: { from: 3765, to: 3810 } },
  ];

  const _traps = options.filter((v) => v.id.includes("trap"));
  const _wins = options.filter((v) => !v.id.includes("trap"));

  console.log(_traps);
  const odds = 0.5;
  const _randomNum = Math.random();

  console.log("odds", odds);
  console.log("random number", _randomNum);

  if (_randomNum < odds) {
    //TODO: Need to randomly select from _trap range.
    console.log("Should lose");
    rotation = 3640;
  } else {
    //TODO: Need to randomly select from _wins range.
    rotation = 3730;
    console.log("Should win");
  }

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
      console.log({ e });
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
    // let duration = Math.floor((e.timeStamp - pressed[e.which]) / 25);
    let duration = 9 || Math.random() * 8 + 2;

    console.log({ duration });

    if (duration > 150) duration = 150;

    animateWheel(spinDuration);
    slowingDown = true;
    pressed[e.which] = 0;
  }

  function animateWheel(duration) {
    // let randomNum = Math.floor(Math.random() * 50);
    // let randomNum = 10;
    // rotation += 30 * duration + 30 * randomNum;
    TweenMax.to(wheel, 10, {
      ease: Power4.easeOut,
      rotation,
      onComplete: resetWheel,
    });
  }

  function resetWheel() {
    slowingDown = false;
    rotation = 0;
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

  <div class="arrow">
    <svg
      width="62"
      height="57"
      viewBox="0 0 62 57"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g filter="url(#filter0_d)">
        <path
          d="M31 49L4.15321 0.250003L57.8468 0.250007L31 49Z"
          fill="#F1AF29"
        />
        <path
          d="M54.4622 2.25001L31 44.854L7.53784 2.25L54.4622 2.25001Z"
          stroke="#033280"
          stroke-width="4"
        />
      </g>
      <defs>
        <filter
          id="filter0_d"
          x="0.153214"
          y="0.25"
          width="61.6936"
          height="56.75"
          filterUnits="userSpaceOnUse"
          color-interpolation-filters="sRGB"
        >
          <feFlood flood-opacity="0" result="BackgroundImageFix" />
          <feColorMatrix
            in="SourceAlpha"
            type="matrix"
            values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"
          />
          <feOffset dy="4" />
          <feGaussianBlur stdDeviation="2" />
          <feColorMatrix
            type="matrix"
            values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"
          />
          <feBlend
            mode="normal"
            in2="BackgroundImageFix"
            result="effect1_dropShadow"
          />
          <feBlend
            mode="normal"
            in="SourceGraphic"
            in2="effect1_dropShadow"
            result="shape"
          />
        </filter>
      </defs>
    </svg>
  </div>

  <div bind:this={wheel} class="wheel">
    {#each options as { id, bg } (id)}
      <Segment value={bg} ref={bg} />
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
    content: "";
    position: absolute;
    left: 50%;
    top: 64%;
    transform: translate(-50%, -50%);
    margin-top: -205px;
    z-index: 3;
    transform-origin: center;
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

    /* box-shadow: 0px 0px 10px 10px #000; */
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
