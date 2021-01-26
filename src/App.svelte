<script>
  import Fortune from "./components/Fortune.svelte";
  const pageModel = {
    uid: "",
    background: "",
    brand: "",
    prehead: {
      text: "",
      styleProps: { textColor: "", fontSize: 0, fontWeight: 0 },
    },
    headerText: {
      text: "",
      styleProps: { textColor: "", fontSize: 0, fontWeight: 0 },
    },
    subHead: {
      text: "",
      styleProps: { textColor: "", fontSize: 0, fontWeight: 0 },
    },
    callToAction: {
      btnText: "",
      type: "http" || "action" || "deeplink",
      styleProps: { textColor: "", fontSize: 0, fontWeight: 0 },
    },
    prizes: [
      { bgImg: "", bgColor: "#ff9fe4", win: { from: 0, to: 0 } },
      { bgImg: "", bgColor: "#009f52", win: { from: 0, to: 0 } },
      { bgImg: "", bgColor: "#ff0000", win: { from: 0, to: 0 } },
      { bgImg: "", bgColor: "orange", win: { from: 0, to: 0 } },
      { bgImg: "", bgColor: "paleturquoise", win: { from: 0, to: 0 } },
      { bgImg: "", bgColor: "purple", win: { from: 0, to: 0 } },
    ],
  };
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
