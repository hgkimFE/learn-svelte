<script lang="ts">
  import { createEventDispatcher, onMount } from "svelte";
  import { selectOnFocus } from "../action";
  import Button from "./Button.svelte";
  const dispatch = createEventDispatcher();
  export let autofocus: boolean = true;

  let name = "";
  let nameEl: HTMLElement;
  let newTodoFormEl: HTMLFormElement;

  const addTodo = () => {
    dispatch("addTodo", name);
    name = "";
    nameEl.focus();
  };

  const onCancel = () => {
    name = "";
    nameEl.focus();
  };
  onMount(() => autofocus && nameEl.focus());
</script>

<form
  bind:this={newTodoFormEl}
  on:submit|preventDefault={addTodo}
  on:keydown={(e) => e.key === "Escape" && onCancel()}
>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">What needs to be done?</label>
  </h2>
  <input
    bind:value={name}
    bind:this={nameEl}
    use:selectOnFocus
    type="text"
    id="todo-0"
    autoComplete="off"
    class="input input__lg"
  />
  <Button
    type={!name ? "disabled" : "tertiary"}
    lg
    disabled={!name}
    on:click={() => {
      newTodoFormEl.dispatchEvent(new Event("submit"));
    }}>Add</Button
  >
</form>
