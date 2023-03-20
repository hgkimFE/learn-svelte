<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let id: string;
  export let checked: boolean;
</script>

<button
  class={`checkbox-container${checked ? " checked" : ""}`}
  on:click={() => dispatch("toggle")}
>
  <input type="checkbox" {id} {checked} />
  <svg viewBox="0 0 20 20">
    <path
      d="M3,1 L17,1 L17,1 C18.1045695,1 19,1.8954305 19,3 L19,17 L19,17 C19,18.1045695 18.1045695,19 17,19 L3,19 L3,19 C1.8954305,19 1,18.1045695 1,17 L1,3 L1,3 C1,1.8954305 1.8954305,1 3,1 Z"
    />
    <polyline points="4 11 8 15 16 6" />
  </svg>
  <slot />
</button>

<style>
  .checkbox-container {
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    font-weight: 400;
    font-size: 1.6rem;
    line-height: 1.25;
    display: block;
    position: relative;
    min-height: 44px;
    padding-left: 40px;
    clear: left;
  }
  .checkbox-container > input[type="checkbox"] {
    -webkit-font-smoothing: antialiased;
    cursor: pointer;
    position: absolute;
    z-index: 1;
    width: 0;
    height: 0;
    margin: 0;
    opacity: 0;
  }
  svg {
    position: absolute;
    left: 0;
    width: 40px;
    height: 40px;
    background-color: var(--disabled-text);
    border-radius: 8px;
  }
  path {
    fill: var(--back);
    stroke: var(--back);
    stroke-width: 2;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 71px;
    stroke-dashoffset: 71px;
    transition: all 0.3s ease;
  }
  polyline {
    fill: none;
    stroke: var(--back);
    stroke-width: 2;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 18px;
    stroke-dashoffset: 18px;
    transition: all 0.3s ease;
  }
  .checkbox-container:hover path {
    stroke-dashoffset: 0;
    stroke: var(--tertiary);
  }
  .checkbox-container.checked svg {
    border: none;
  }
  input[type="checkbox"]:checked + svg > path {
    stroke-dashoffset: 0;
    fill: var(--tertiary);
    stroke: var(--tertiary);
  }
  input[type="checkbox"]:checked + svg > polyline {
    stroke-dashoffset: 0;
  }
</style>
