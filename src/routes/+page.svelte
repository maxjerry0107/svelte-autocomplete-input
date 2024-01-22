<script lang="ts">
  import AutoCompleteInput from "$lib/AutoCompleteInput.svelte";
  const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];

  let color: string;
  function handleChange(e: Event): void {
    console.log(color);
  }
</script>

<AutoCompleteInput
  name="color"
  containerStyle="width:100px"
  placeholder="Colors"
  items={colors}
  on:change={handleChange}
  on:blur={handleChange}
  bind:value={color}
/>
