<script lang="ts">
  import TodosStatus from "./TodosStatus.svelte";
  import FilterButton from "./FilterButton.svelte";
  import MoreActions from "./MoreActions.svelte";
  import NewTodo from "./NewTodo.svelte";
  import Todo from "./Todo.svelte";
  import { alert } from "../stores";
  import type { TodoType } from "../types/todo.type";
  import { Filter } from "../types/filter.enum";

  export let todos: TodoType[] = [];
  let todosStatus: TodosStatus;

  $: newTodoId = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;

  function addTodo(name: string) {
    todos = [...todos, { id: newTodoId, name, completed: false }];
    $alert = `Todo '${name}' has been added`;
  }
  function updateTodo(todo: TodoType) {
    const i = todos.findIndex((t) => t.id === todo.id);
    if (todos[i].name !== todo.name)
      $alert = `todo '${todos[i].name}' has benn renamed to '${todo.name}'`;
    if (todos[i].completed !== todo.completed)
      $alert = `todo '${todos[i].name}' marked as '${
        todo.completed ? "completed" : "active"
      }'`;
    todos[i] = { ...todos[i], ...todo };
  }
  function removeTodo(todo: TodoType) {
    todos = todos.filter((t) => t.id !== todo.id);
    todosStatus.focus();
    $alert = `Todo '${todo.name}' has been deleted`;
  }

  let filter: Filter = Filter.ALL;

  $: {
    if (filter === Filter.ALL) {
      $alert = "Browsing all to-dos";
    } else if (filter === Filter.ACTIVE) {
      $alert = "Browsing active to-dos";
    } else if (filter === Filter.COMPLETED) {
      $alert = "Browsing completed to-dos";
    }
  }

  const filterTodos = (filter: Filter, todos: TodoType[]) =>
    filter === Filter.ACTIVE
      ? todos.filter((t) => !t.completed)
      : filter === Filter.COMPLETED
      ? todos.filter((t) => t.completed)
      : todos;

  const checkAllTodos = (completed: boolean) => {
    todos = todos.map((t) => ({ ...t, completed }));
    $alert = `${completed ? "Checked" : "Unchecked"} ${todos.length} to-dos`;
  };
  const removeCompletedTodos = () => {
    $alert = `Removed ${todos.filter((t) => t.completed).length} to-dos`;
    todos = todos.filter((t) => !t.completed);
  };
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <NewTodo on:addTodo={(e) => addTodo(e.detail)} />
  <FilterButton bind:filter />
  <TodosStatus bind:this={todosStatus} {todos} />
  <ul class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <Todo
          {todo}
          on:update={(e) => updateTodo(e.detail)}
          on:remove={(e) => removeTodo(e.detail)}
        />
      </li>
    {:else}
      Nothing to do here!
    {/each}
  </ul>
  <hr />
  <MoreActions
    {todos}
    on:checkAll={(e) => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />
</div>
