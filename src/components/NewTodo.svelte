<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import Button from "./Button.svelte";
  import TextInput from "./TextInput.svelte";
  const dispatch = createEventDispatcher();

  let name = "";
  let newTodoFormEl: HTMLFormElement;
  let nameEl: HTMLElement;
  const addTodo = () => {
    dispatch("addTodo", name);
    name = "";
    nameEl.focus();
  };

  function changeName(e) {
    name = e.detail;
  }
</script>

<form bind:this={newTodoFormEl} on:submit|preventDefault={addTodo}>
  <h2 class="label-wrapper">
    <!-- <label for="todo-0" class="label__lg">
    </label> -->
    What needs to be done?
  </h2>
  <TextInput
    value={name}
    on:change={changeName}
    on:save={addTodo}
    bind:nameEl
  />
  <Button
    type={!name ? "disabled" : "tertiary"}
    lg
    disabled={!name}
    attr={{ style: "transition: background-color 0.2s" }}
    on:click={() => {
      newTodoFormEl.dispatchEvent(new Event("submit"));
    }}>Add</Button
  >
</form>
