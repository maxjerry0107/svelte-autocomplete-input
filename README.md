# Auto Complete Input

Autocomplete input made with [Svelte](https://svelte.dev/).

## Install

```bash
npm install -D svelte-autocomplete-input
```

## Usage

Import the component and define items:

```javascript
import AutoCompleteInput from "svelte-autocomplete-input"

const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"]
let selectedColor

<AutoCompleteInput
  name="color"
  placeholder="Colors"
  items={colors}
  on:change={handleChange}
  on:blur={handleChange}
  bind:value={selectedColor}
/>

```

Can be used with tailwindcss.

```javascript
<AutoCompleteInput
  name="color"
  placeholder="Colors"
  items={colors}
  on:change={handleChange}
  on:blur={handleChange}
  bind:value={selectedColor}
  inputClass="rounded border p-3"
  itemClass="border border-t-0 py-3 px-2 text-left"
/>
```

## Properties

- `items` - array of items the user can select from
- `placeholder` - placeholder of the input tag
- `value` - selected value
- `defaultValue` - default value
- `searchMode` - "include" | "start"
- `name` - name of the input tag
- `containerClass` - container class
- `inputClass` - input class
- `itemClass` - item class
- `containerStyle` - container style
- `inputStyle` - input style
- `itemStyle` - item style
- `itemBackColor` - item background color. default(#ffffff)
- `itemColor` - item text color. default(#000000)
- `selectedItemBackColor` - selected item background color. default(#000000)
- `selectedItemColor` - selected item text color. default(#ffffff)
