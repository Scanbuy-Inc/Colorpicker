<script>
  import Colorpicker from "./Colorpicker.svelte"
  import CheckedBackground from "./CheckedBackground.svelte"
  import {createEventDispatcher, beforeUpdate, onMount} from "svelte"

  import {buildStyle, debounce} from "../helpers.js"
  import {fade} from "svelte/transition"
  import {getColorFormat} from "../utils.js"

  export let value = "#3ec1d3ff"
  export let swatches = []
  export let disableSwatches = false
  export let open = false
  export let width = "25px"
  export let height = "25px"

  let format = "hexa"
  let dimensions = {top: 0, bottom: 0, right: 0, left: 0}
  let positionY = "top"
  let positionX = "left"
  let colorPreview = null

  let previewHeight = null
  let previewWidth = null
  let pickerWidth = 0
  let pickerHeight = 0

  let errorMsg = null

  const dispatch = createEventDispatcher()
  const dispatchValue = value =>
          debounce(() => dispatch("change", value), 300, true)

  beforeUpdate(() => {
    format = getColorFormat(value)
    if (!format) {
      errorMsg = `Colorpicker - ${value} is an unknown color format. Please use a hex, rgb or hsl value`
      console.error(errorMsg)
    } else {
      errorMsg = null
    }
  })

  function openColorpicker(event) {
    if (colorPreview) {
      open = true
    }
  }

  function onColorChange(color) {
    value = color.detail
    dispatchValue(value)
  }

  function calculateDimensions() {
    if (open) {
      const {
        top: spaceAbove,
        width,
        bottom,
        right,
        left,
      } = colorPreview.getBoundingClientRect()

      const spaceBelow = window.innerHeight - bottom
      const previewCenter = previewWidth / 2

      let y, x

      if (spaceAbove > spaceBelow) {
        positionY = "bottom"
        y = window.innerHeight - spaceAbove
      } else {
        positionY = "top"
        y = bottom
      }

      // Centre picker by default
      x = left + previewCenter - 220 / 2

      const spaceRight = window.innerWidth - right

      //Position picker left or right if space not available for centering
      if (left < 110 && spaceRight > 220) {
        positionX = "left"
        x = right
      } else if (spaceRight < 100 && left > 220) {
        positionX = "right"
        x = document.body.clientWidth - left
      }

      dimensions = {[positionY]: y.toFixed(1), [positionX]: x.toFixed(1)}
    }
  }

  $: if (open && colorPreview) {
    calculateDimensions()
  }

  $: previewStyle = buildStyle({width, height, background: value})
  $: errorPreviewStyle = buildStyle({width, height})
  $: pickerStyle = buildStyle({
    [positionY]: `${dimensions[positionY]}px`,
    [positionX]: `${dimensions[positionX]}px`,
  })
</script>

<svelte:window on:resize={debounce(calculateDimensions, 200)}/>

<div className="color-preview-container">
  {#if !errorMsg}
    <CheckedBackground borderRadius="3px" backgroundSize="8px">
      <div class="container">
        <div class="color-container" on:click={openColorpicker}>
          <span>{value}</span>
          <div
              bind:this={colorPreview}
              bind:clientHeight={previewHeight}
              bind:clientWidth={previewWidth}
              title={value}
              className="color-preview"
              style={previewStyle}
              />
        </div>
      </div>
    </CheckedBackground>

    {#if open}
      <Colorpicker
              style={pickerStyle}
              on:change={onColorChange}
              on:addswatch
              on:removeswatch
              bind:format
              bind:value
              bind:pickerHeight
              bind:pickerWidth
              bind:open
              {swatches}
              {disableSwatches}/>
      <div on:click|self={() => (open = false)} class="overlay"/>
    {/if}
  {:else}
    <div
            class="color-preview preview-error"
            title="Invalid Color"
            style={errorPreviewStyle}>
      <span>&times;</span>
    </div>
  {/if}
</div>

<style>
  .color-preview-container {
    display: flex;
    flex-flow: row nowrap;
    height: fit-content;
  }

  .color-preview {
    cursor: pointer;
    border-radius: 3px;
    border: 1px solid #dedada;
  }

  .preview-error {
    background: #cccccc;
    color: #808080;
    text-align: center;
    font-size: 18px;
    cursor: not-allowed;
  }

  .overlay {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 2;
  }

  .container{
    width: 270.419px;
    height: 40px;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    background: white;
    cursor: pointer;
  }

  .color-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem;
    border-radius: 15px;
  }
</style>
