<script lang="ts">
  import { createEventDispatcher, tick } from "svelte";
  import { selectOnFocus } from "../action";
  import type { TodoType } from "../types/todo.type";
  import Button from "./Button.svelte";
  import CheckBox from "./CheckBox.svelte";
  export let todo: TodoType;
  let editing = false;
  let name = todo.name;
  let nameEl: HTMLElement;
  let renameFormEl: HTMLFormElement;
  let editButtonPressed = false;

  function update(updatedTodo: Partial<TodoType>) {
    todo = { ...todo, ...updatedTodo };
    dispatch("update", todo);
  }
  function onCancel() {
    name = todo.name;
    editing = false;
  }

  function onSave() {
    update({ name });
    editing = false;
  }

  function onRemove() {
    dispatch("remove", todo);
  }

  const focusOnInit = (node) =>
    node && typeof node.focus === "function" && node.focus();

  async function onEdit() {
    editButtonPressed = true;
    editing = true;
    await tick();
    nameEl.focus();
  }

  function onToggle() {
    console.log("ontoggle");
    update({ completed: !todo.completed });
  }
  function preventEnter(e) {
    if (e.key == "Enter") {
      e.preventDefault();
      onSave();
    }
  }
  const dispatch = createEventDispatcher();
</script>

<div class="stack-small">
  {#if editing}
    <form
      bind:this={renameFormEl}
      on:submit|preventDefault={onSave}
      class="stack-small"
      on:keydown={(e) => e.key === "Escape" && onCancel()}
    >
      <div class="form-group">
        <label for="todo-{todo.id}" class="todo-label"
          >New name for '{todo.name}'</label
        >
        <input
          bind:value={name}
          bind:this={nameEl}
          use:selectOnFocus
          use:focusOnInit
          on:keydown={preventEnter}
          placeholder="test"
          type="text"
          id="todo-{todo.id}"
          autoComplete="off"
          class="todo-text"
        />
      </div>
      <div class="btn-group">
        <Button on:click={onCancel}
          >Cancel<span class="visually-hidden">renaming {todo.name}</span
          ></Button
        >
        <Button
          type="tertiary"
          on:click={() => {
            renameFormEl.dispatchEvent(new Event("submit"));
          }}
          disabled={!name}
          >Save<span class="visually-hidden">new name for {todo.name}</span
          ></Button
        >
      </div>
    </form>
  {:else}
    <!-- <div class="c-cb"> -->
    <CheckBox
      id={`todo-${todo.id}`}
      checked={todo.completed}
      on:toggle={onToggle}
    >
      <label for="todo-{todo.id}" class="todo-label">{todo.name}</label>
    </CheckBox>
    <!-- <input
        type="checkbox"
        id="todo-{todo.id}"
        on:click={onToggle}
        checked={todo.completed}
      />
      <label for="todo-{todo.id}" class="todo-label">{todo.name}</label> -->
    <!-- </div> -->
    <div class="btn-group">
      <Button focused={editButtonPressed} on:click={onEdit}
        >Edit<span class="visually-hidden"> {todo.name}</span></Button
      >
      <Button type="primary" on:click={onRemove}
        >Delete<span class="visually-hidden"> {todo.name}</span></Button
      >
    </div>
  {/if}
</div>

<style>
  label {
    font-size: inherit;
    font-family: inherit;
    line-height: inherit;
    display: inline-block;
    margin-bottom: 0;
    padding: 8px 15px 5px;
    cursor: pointer;
    touch-action: manipulation;
  }
</style>
