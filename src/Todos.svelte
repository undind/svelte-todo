<script>
  import TodoItem from "./TodoItem.svelte";

  let newTodoTitle = "";
  let curentFilter = "All";
  let nextId = 4;

  let todos = [
    {
      id: 1,
      title: "My first todo",
      completed: false
    },
    {
      id: 2,
      title: "My second todo",
      completed: false
    },
    {
      id: 3,
      title: "My third todo",
      completed: false
    }
  ];

  function addTodo(event) {
    if (event.key === "Enter") {
      todos = [
        ...todos,
        {
          id: nextId,
          completed: false,
          title: newTodoTitle
        }
      ];

      nextId = nextId + 1;
      newTodoTitle = "";
    }
  }

  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
  $: filteredTodos =
    currentFilter === "all"
      ? todos
      : currentFilter === "completed"
      ? todos.filter(todo => todo.completed)
      : todos.filter(todo => !todo.completed);

  function checkAllTodos(event) {
    todos.forEach(todo => (todo.completed = event.target.checked));
    todos = todos;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter(todo => !todo.completed);
  }

  function handleDeleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id);
  }
</script>

<div class="div container">
  <h2>Svelte Todo App</h2>
  <input
    type="text"
    class="todo-input"
    placeholder="Insert todo item..."
    bind:value={newTodoTitle}
    on:keydown={addTodo} />

  {#each filteredTodos as todo}
    <div class="todo-item">
      <TodoItem
        {...todo}
        on:deleteTodo={handleDeleteTodo}
        on:toggleComplete={handleToggleComplete} />
    </div>
  {/each}

  <div class="inner-container">
    <div>
      <label>
        <input
          class="inner-container-input"
          type="checkbox"
          on:change={checkAllTodos} />
        Check All
      </label>
    </div>
  </div>

  <div>{todosRemaining} items left</div>

  <div class="inner-container">
    <div>
      <button
        on:click={() => updateFilter('all')}
        class:active={curentFilter === 'all'}>
        All
      </button>
      <button
        on:click={() => updateFilter('active')}
        class:active={curentFilter === 'active'}>
        Active
      </button>
      <button
        on:click={() => updateFilter('completed')}
        class:active={curentFilter === 'completed'}>
        Completed
      </button>
    </div>

    <div>
      <button on:click={clearCompleted}>Clear Completed</button>
    </div>
  </div>

</div>
