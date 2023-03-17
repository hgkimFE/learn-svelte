<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import type { TodoType } from "../types/todo.type";
  import Button from "./Button.svelte";
  const dispatch = createEventDispatcher();

  export let todos: TodoType[];

  let completed = true;

  const checkAll = () => {
    dispatch("checkAll", completed);
    completed = !completed;
  };

  const removeCompleted = () => dispatch("removeCompleted");

  $: completedTodos = todos.filter((t) => t.completed).length;
</script>

<div class="btn-group">
  <Button disabled={todos.length === 0} on:click={checkAll}
    >{completed ? "Check" : "Uncheck"} all</Button
  >
  <Button
    type="primary"
    disabled={completedTodos === 0}
    on:click={removeCompleted}>Remove completed</Button
  >
</div>
