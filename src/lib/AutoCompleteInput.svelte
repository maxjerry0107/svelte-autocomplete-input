<script lang="ts">
  import { onMount } from "svelte";

  export let placeholder: string = "";
  export let items: string[] = [];
  export let value;
  export let defaultValue = "";
  export let searchMode: "include" | "start" = "start";
  export let name: string | null = null;

  export let containerClass: string = "";
  export let inputClass: string = "";
  export let itemClass: string = "";

  export let containerStyle: string = "";
  export let inputStyle: string = "";
  export let itemStyle: string = "";

  export let itemBackColor: string = "#ffffff";
  export let itemColor: string = "#000000";
  export let selectedItemBackColor: string = "#000000";
  export let selectedItemColor: string = "#ffffff";

  let filteredItems: string[] = [];
  let searchInputRef: HTMLInputElement;
  let autocompleteRef: HTMLDivElement;
  let inputValue = "";

  onMount(() => {
    autocompleteRef.addEventListener("keydown", navigateList);
    return () => autocompleteRef.removeEventListener("keydown", navigateList);
  });

  const filterItems = () => {
    let storageArr: string[] = [];
    if (inputValue) {
      items.forEach((item) => {
        if (searchMode == "include") {
          if (item.toLowerCase().includes(inputValue.toLowerCase())) {
            storageArr = [...storageArr, makeMatchBold(item)];
          }
        } else {
          if (item.toLowerCase().startsWith(inputValue.toLowerCase())) {
            storageArr = [...storageArr, makeMatchBold(item)];
          }
        }
      });
    }
    filteredItems = storageArr;
  };

  $: if (!inputValue) {
    filteredItems = [];
    hiLiteIndex = -1;
  }

  const setInputVal = (item: string) => {
    inputValue = removeBold(item);
    filteredItems = [];
    hiLiteIndex = -1;
    searchInputRef.focus();
  };

  const makeMatchBold = (str: string) => {
    if (searchMode == "include") {
      const regEx = new RegExp(inputValue, "gi");
      let boldedMatch = str.replace(regEx, "<strong>$&</strong>");
      return boldedMatch;
    } else {
      let matched = str.substring(0, inputValue.length);
      let makeBold = `<strong>${matched}</strong>`;
      let boldedMatch = str.replace(matched, makeBold);
      return boldedMatch;
    }
  };

  const removeBold = (str: string) => {
    return str.replace(/<(.)*?>/g, "");
  };

  let hiLiteIndex: number = -1;

  $: value = filteredItems[hiLiteIndex] || inputValue;

  $: inputValue = defaultValue;

  const navigateList = (e: KeyboardEvent) => {
    if (e.key === "ArrowDown" && hiLiteIndex <= filteredItems.length - 1) {
      hiLiteIndex === null ? (hiLiteIndex = 0) : (hiLiteIndex += 1);
    } else if (e.key === "ArrowUp" && hiLiteIndex !== null) {
      hiLiteIndex === 0
        ? (hiLiteIndex = filteredItems.length - 1)
        : (hiLiteIndex -= 1);
    } else if (e.key === "Enter") {
      setInputVal(filteredItems[hiLiteIndex]);
    } else {
      return;
    }
  };
</script>

<div
  bind:this={autocompleteRef}
  style={containerStyle}
  class={`autocomplete ${containerClass}`}
>
  <input
    {name}
    {...$$restProps}
    class={inputClass}
    style={inputStyle}
    type="text"
    on:change
    on:blur
    {placeholder}
    bind:this={searchInputRef}
    bind:value={inputValue}
    on:input={filterItems}
  />
  <div
    class="autocomplete-list-container"
    style="--item-bg:{itemBackColor}; --item-color:{itemColor};--selected-item-bg:{selectedItemBackColor}; --selected-item-color:{selectedItemColor};"
  >
    {#if filteredItems.length > 0}
      <div class="autocomplete-list">
        {#each filteredItems as item, i}
          <button
            on:click={() => setInputVal(item)}
            class={`autocomplete-list-item ${itemClass}`}
            class:selected={i === hiLiteIndex}
            style={itemStyle}
          >
            {@html item}
          </button>
        {/each}
      </div>
    {/if}
  </div>
</div>

<style>
  .autocomplete {
    width: 100%;
  }
  .autocomplete input {
    display: block;
    width: 100%;
  }
  .autocomplete input:focus-visible {
    outline-width: 0px;
  }
  .autocomplete-list-container {
    position: relative;
  }
  .autocomplete-list {
    position: absolute;
    margin: 0;
    max-height: 192px;
    width: 100%;
    overflow-y: auto;
    padding: 0;
  }
  .autocomplete-list-item {
    background-color: var(--item-bg);
    color: var(--item-color);
    width: 100%;
    height: 100%;
    text-align: inherit;
    cursor: pointer;
    appearance: none;
  }
  .autocomplete-list-item.selected {
    background-color: var(--selected-item-bg);
    color: var(--selected-item-color);
  }
  .autocomplete-list-item:hover {
    background-color: var(--selected-item-bg);
    color: var(--selected-item-color);
  }
</style>
