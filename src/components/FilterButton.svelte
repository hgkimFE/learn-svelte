<script lang="ts">
  import { Filter } from "../types/filter.enum";
  import Button from "./Button.svelte";

  export let filter: Filter = Filter.ALL;
</script>

<div class="filters btn-group stack-exception">
  <Button
    type={filter === Filter.ALL ? "disabled" : ""}
    disabled={filter === Filter.ALL}
    ariaPressed={filter === Filter.ALL}
    on:click={() => (filter = Filter.ALL)}
  >
    <span class="visually-hidden">Show</span>
    <span>All</span>
    <span class="visually-hidden">tasks</span>
  </Button>
  <Button
    type={filter === Filter.ACTIVE ? "disabled" : ""}
    disabled={filter === Filter.ACTIVE}
    ariaPressed={filter === Filter.ACTIVE}
    on:click={() => (filter = Filter.ACTIVE)}
  >
    <span class="visually-hidden">Show</span>
    <span>Active</span>
    <span class="visually-hidden">tasks</span>
  </Button>
  <Button
    type={filter === Filter.COMPLETED ? "disabled" : ""}
    disabled={filter === Filter.COMPLETED}
    ariaPressed={filter === Filter.COMPLETED}
    on:click={() => (filter = Filter.COMPLETED)}
  >
    <span class="visually-hidden">Show</span>
    <span>Completed</span>
    <span class="visually-hidden">tasks</span>
  </Button>
</div>
