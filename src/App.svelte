<script lang="ts">
  import { IroColor } from "@irojs/iro-core";
  import iro from "@jaames/iro";
  import { onMount } from "svelte";

  let color: iro.Color = new IroColor("#000000");
  $: style = `background-color: ${color.hexString}; 
    color: ${(color.hsv.v || 0) < 75 ? "white" : "black"}`;
  let copying = false;

  let colorPicker: iro.ColorPicker;
  onMount(() => {
    colorPicker = iro.ColorPicker("#picker", {
      color: color.hexString,
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
      color = nextColor;
    });
  });

  function copy() {
    navigator.clipboard.writeText(color?.hexString);
    copying = true;
    setTimeout(() => (copying = false), 500);
  }
</script>

<main>
  <div id="picker"></div>
  <div id="select">
    <label for="color">Selected Color: {color.hexString}</label>
    <div>
      <button id="color" {style} on:click={copy}>
        <h1>{color.hexString}</h1>
        {#if copying}
          <h3>Copied!</h3>
        {:else}
          <h3>Copy to Clipboard</h3>
        {/if}
      </button>
    </div>
  </div>
</main>

<style>
  button {
    width: 400px;
    border: 1px;
    border: solid;
    border-color: white;
    border-radius: 14px;
    border-width: 1px;
    text-align: center;
    color: white;
    cursor: pointer;
  }

  #select {
    margin-top: 1em;
  }
</style>
