<script lang="ts">
  import { onMount, createEventDispatcher } from "svelte";
  import { selectOnFocus } from "../action";
  const dispatch = createEventDispatcher();

  export let value: string;
  $: dispatch("change", value);
  let autofocus: boolean = true;
  let nameEl: HTMLElement;
  const onCancel = () => {
    value = "";
    nameEl.focus();
  };
  function onKeyDown(e: KeyboardEvent) {
    if (e.key === "Escape") onCancel();
    else if (e.key == "Enter") {
      console.log("enter");
      e.preventDefault();
      dispatch("save");
    }
  }
  onMount(() => autofocus && nameEl.focus());
</script>

<div class="input-container __lg">
  <input
    bind:value
    bind:this={nameEl}
    use:selectOnFocus
    on:keydown={onKeyDown}
    id="input-text"
    type="text"
    autoComplete="off"
    class="todo-text"
    placeholder=" "
  />
  <label for="input-text">Add a task to your list.</label>
</div>

<style>
  .input-container {
    position: relative;
  }
  .input-container::after {
    content: "";
    position: absolute;
    left: 2rem;
    bottom: 1.8rem;
    width: calc(100% - 4rem);
    height: 1px;
    background-color: var(--disabled-text);
    transition: all 0.2s ease;
  }
  .input-container:focus-within::after {
    background-color: var(--heading);
  }
  .todo-text {
    width: 100%;
    min-height: 4.4rem;
    padding: 2rem;
    border: none;
    border-radius: 2px;
    box-shadow: 0.2rem 0.2rem rgba(0, 0, 0, 0.1);
  }
  .input-container label {
    color: var(--disabled-text);
    position: absolute;
    top: 2rem;
    left: 2.1rem;
    pointer-events: none;
    transition: all 0.2s ease;
  }
  .todo-text:not(:placeholder-shown) + label {
    transform: translateY(-2rem);
    font-size: 1.6rem;
    color: var(--tertiary);
  }
</style>
