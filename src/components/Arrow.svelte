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

<style>
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
  @media (max-width: 320px) {
    .arrow {
      transform: translate(-50%, 10%) scale(0.7);
    }
  }
</style>
