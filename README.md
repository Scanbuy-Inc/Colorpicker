# Colorpicker 🎨
A multi format Colorpicker built with Svelte. The Colorpicker accepts and sets hex(a), rgb(a) and hsl(a) colors.

![Image](./img/colorpicker.png)

### Features
* Choose your color by clicking or dragging on a palette.
* Set the hue and opacity of your chosen color using Slider controls.
* Convert your selected color to your preferred color format (hexa, rgba, hsla).
* Set your color by typing into an input.
* A swatch panel for keeping track of recent colours or for passing favourite colors so they can be easily set again.
* Handy keyboard events to tab through control panel, set colors and close the Colorpicker.
* Colorpicker anchors to its preview element which displays the selected color and acts as a button to display the Colorpicker.
* Viewport positional awareness - the colorpicker centers by default but will adapt and position itself appropriately based on available space.
* Colorpicker renders outside the dom structure which prevents problems with overflow clipping in scrolling containers.

### Events
The Colorpicker exposes the following events. In each case, the color will be provided as the first parameter to the bound function.

**Change Event**
`on:change={selectedColor: string => {}}` 
The on change event will be invoked whenever a color has been set in the Colorpicker via the palette, the hue slider, the alpha slider or by typing into the input.

**Add Swatch**
`on:addswatch={addedSwatch: string => {}}`
The add swatch event will be invoked when a user adds a swatch by clicking the add button.

**Remove Swatch**
`on:removeswatch={removedSwatch: string => {}}`
The remove swatch will be invoked when a user clicks the delete button that appears when hovering over a swatch.

### Swatches
* By default, added swatches are saved to local storage so that they can be displayed across all Colorpickers in your application. 
* Only 12 or less swatches can be displayed in the Coloricker at any any one time. 
* If more than 12 have been passed the Colorpicker will display the first 12 and warn in the console.
* An array of swatches can be passed to the Colorpicker to set up a dedicated panel of swatches. This will use provided swatches instead of locally stored swatches. Swatches added will still be saved to local storage.
* You can disable swatch functionality by passing the `disableSwatches={true}` property to the Colorpicker.

This is a readme for your new Budibase plugin.

Find out more about [Budibase](https://github.com/Budibase/budibase).

## Instructions

To build your new  plugin run the following in your Budibase CLI:
```
budi plugins --build
```

You can also re-build everytime you make a change to your plugin with the command:
```
budi plugins --watch
```



