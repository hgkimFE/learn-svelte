<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let type = "";
  export let disabled = false;
  export let focused = false;
  export let lg = false;
  export let ariaPressed: undefined | boolean = undefined;
  export let attr: undefined | object = undefined;

  $: isFocused = focused;
  $: cssBtnType = `${
    disabled ? "btn--disabled" : type.length ? "btn--" + type : ""
  }${lg ? "__lg" : ""}`;
  const onClick = () => dispatch("click");
  const focusButton = (node) => isFocused && node.focus();
</script>

<button
  class="btn {cssBtnType}"
  {disabled}
  aria-pressed={ariaPressed}
  {...attr}
  on:click={onClick}
  use:focusButton><slot /></button
>

<style>
  .btn {
    padding: 0.8rem 1rem 0.7rem;
    border: none;
    border-radius: 2px;
    cursor: pointer;
    text-transform: capitalize;
    box-shadow: 0.2rem 0.2rem rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
  }
  .btn:hover {
    transform: translateY(-2px);
  }
  .btn:focus {
    outline: none;
    box-shadow: 0 0 0 4px rgba(33, 150, 243, 0.7);
  }
  .btn[aria-pressed="true"] {
    text-decoration: underline;
  }
  [class*="--disabled"]:hover {
    transform: none;
    box-shadow: 0.2rem 0.2rem rgba(0, 0, 0, 0.1);
    cursor: not-allowed;
  }
</style>
