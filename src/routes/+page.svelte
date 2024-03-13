<script lang="ts">
  import AutoCompleteInput from "$lib/AutoCompleteInput.svelte";
  import type { ComponentEvents } from "svelte";

  import HighlightSvelte, { LineNumbers } from "svelte-highlight";
  import typescript from "svelte-highlight/languages/typescript";
  import irBlack from "svelte-highlight/styles/ir-black";
  const code = `
  <script lang="ts">
    import AutoCompleteInput from "$lib/AutoCompleteInput.svelte";
    const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];

    function handleChange(event: ComponentEvents<AutoCompleteInput>['onChangeValue']): void {
      console.log(event.detail);
    }
  <\/script>

  <AutoCompleteInput
    name="color"
    containerStyle="width:100px"
    placeholder="Colors"
    items={colors}
    on:onChangeValue={handleChange}
  />
  `;
  const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];

  function handleChange(event: ComponentEvents<AutoCompleteInput>['onChangeValue']): void {
    console.log(event.detail);
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
    on:onChangeValue={handleChange}
  />
</div>

<style>
  .container {
    width: 50%;
  }
</style>
