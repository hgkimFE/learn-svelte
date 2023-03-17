<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let type = "";
  export let disabled = false;
  $: cssBtnType = disabled
    ? "btn--disabled"
    : type.length
    ? "btn--" + type
    : "";
  const onClick = () => dispatch("click");
</script>

<button class="btn {cssBtnType}" {disabled} on:click={onClick}><slot /></button>

<style>
  .btn {
    padding: 0.8rem 1rem 0.7rem;
    border: none;
    border-radius: 2px;
    cursor: pointer;
    text-transform: capitalize;
    box-shadow: 0.2rem 0.2rem rgba(0, 0, 0, 0.1);
    transition: transform 0.2s, box-shadow 0.2s, background-color 0.1s;
  }
  .btn:hover {
    transform: translateY(-2px);
    box-shadow: 0.3rem 0.4rem rgba(0, 0, 0, 0.1);
  }
  .btn:focus {
    outline: none;
    box-shadow: 0 0 0 4px rgba(33, 150, 243, 0.4);
  }
  .btn--primary {
    color: var(--sidebar-text);
    background-color: var(--prime);
  }
  .btn--secondary {
    color: var(--sidebar-text);
    background-color: var(--second);
  }
  .btn--flash {
    color: var(--sidebar-text);
    background-color: var(--flash);
  }
  .btn--disabled {
    color: var(--sidebar-text);
    background-color: var(--second-text);
  }
</style>
