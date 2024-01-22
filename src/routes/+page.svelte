<script lang="ts">
  import AutoCompleteInput from "$lib/AutoCompleteInput.svelte";

  import HighlightSvelte, { LineNumbers } from "svelte-highlight";
  import typescript from "svelte-highlight/languages/typescript";
  import irBlack from "svelte-highlight/styles/ir-black";
  const code = `
  <script lang="ts">
    import AutoCompleteInput from "$lib/AutoCompleteInput.svelte";
    const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];

    let color: string;
    function handleChange(e: Event): void {
      console.log(color);
    }
  <\/script>

  <AutoCompleteInput
    name="color"
    containerStyle="width:100px"
    placeholder="Colors"
    items={colors}
    on:blur={handleChange}
    bind:value={color}
  />
  `;
  const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];

  let color: string;
  function handleChange(e: Event): void {
    console.log(color);
  }
</script>

<svelte:head>
  {@html irBlack}
</svelte:head>

<div class="container">
  <HighlightSvelte language={typescript} {code} let:highlighted>
    <LineNumbers {highlighted} />
  </HighlightSvelte>

  <br />

  <AutoCompleteInput
    name="color"
    containerStyle="width:100px"
    placeholder="Colors"
    items={colors}
    on:blur={handleChange}
    bind:value={color}
  />
</div>

<style>
  .container {
    width: 50%;
  }
</style>
