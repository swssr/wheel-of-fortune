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
      deeplink: "",
      http: "",
      intent: "",
      styleProps: { textColor: "", fontSize: 0, fontWeight: 0 },
    },
    spinDuration: 3,
    prizes: [
      { bgImg: "", bgColor: "#ff9fe4", win: { from: 1, to: 3 } },
      { bgImg: "", bgColor: "#009f52", win: { from: 3, to: 5 } },
      { bgImg: "", bgColor: "#ff0000", win: { from: 5, to: 7 } },
      { bgImg: "", bgColor: "orange", win: { from: 7, to: 9 } },
      { bgImg: "", bgColor: "paleturquoise", win: { from: 9, to: 11 } },
      { bgImg: "", bgColor: "purple", win: { from: 11, to: 13 } },
    ],
  };
</script>

<main>
  <Fortune spinDuration={pageModel.spinDuration} />
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    text-align: center;
    padding: 2em;
    background: url(https://res.cloudinary.com/tumi/image/upload/v1611664122/game-bg.jpg)
      no-repeat;
    background-size: cover;
    background-position: -1% -1%;
    height: 100%;
    width: 100%;
  }
</style>
