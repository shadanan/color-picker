<script lang="ts">
  import iro from "@jaames/iro";
  import { onMount } from "svelte";

  let color: string = "#000000";

  let colorPicker: iro.ColorPicker;
  onMount(() => {
    colorPicker = iro.ColorPicker("#picker", {
      color,
      width: 400,
      borderWidth: 1,
      layout: [
        {
          component: iro.ui.Box,
        },
        {
          component: iro.ui.Slider,
          options: {
            sliderType: "hue",
          },
        },
      ],
    });

    colorPicker.on("color:change", (nextColor: iro.Color) => {
      color = nextColor.hexString;
    });
  });

  function copy() {
    navigator.clipboard.writeText(color);
  }
</script>

<main>
  <div id="picker"></div>
  <div id="select">
    <label for="color">Selected Color</label>
    <div id="color" style="background-color: {color};">
      <h1>{color}</h1>
    </div>
  </div>
</main>

<style>
  #select {
    margin-top: 1em;
  }

  #color {
    width: 400px;
    border: 1px;
    border: solid;
    border-color: white;
    border-radius: 14px;
    border-width: 1px;
    text-align: center;
    color: white;
    -webkit-text-stroke: 1px black;
    font-family: "Roboto Mono", "Courier New", Courier, monospace;
  }
</style>
