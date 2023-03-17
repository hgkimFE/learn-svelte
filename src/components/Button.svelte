<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let type = "";
  export let disabled = false;
  export let focused = false;
  export let lg = false;
  export let ariaPressed: undefined | boolean = undefined;
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
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .btn:hover {
    transform: translateY(-2px);
    box-shadow: 0.3rem 0.4rem rgba(0, 0, 0, 0.1);
  }
  .btn:focus {
    outline: none;
    box-shadow: 0 0 0 4px rgba(33, 150, 243, 0.4);
  }
  [class*="--primary"] {
    color: var(--sidebar-text);
    background-color: var(--prime);
  }
  [class*="--secondary"] {
    color: var(--sidebar-text);
    background-color: var(--second);
  }
  [class*="--tertiary"] {
    color: var(--sidebar-text);
    background-color: var(--tertiary);
  }
  [class*="--disabled"] {
    color: var(--sidebar-text);
    background-color: var(--second-text);
  }
  [class*="--disabled"]:hover {
    transform: none;
    box-shadow: 0.2rem 0.2rem rgba(0, 0, 0, 0.1);
    cursor: not-allowed;
  }
  [class*="__lg"] {
    width: 100%;
    display: inline-block;
    font-size: 2.4rem;
  }
</style>
