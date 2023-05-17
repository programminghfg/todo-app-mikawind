<script>
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
    import { writable, get } from 'svelte/store';
    let todoText = '';
    let todos = writable([]);
    onMount(() => {
      if (browser) {
        const storedTodos = getTodosFromLocalStorage();
        if (storedTodos) {
          todos.set(storedTodos);
        }
      }
    });
    function saveTodos() {
      if (browser) {
        localStorage.setItem('todos', JSON.stringify(get(todos)));
      }
    }
    function addTodo() {
      if (todoText.trim() !== '') {
        todos.update(items => {
          const updatedItems = [...items, { text: todoText, done: false }];
          saveTodos();
          return updatedItems;
        });
        todoText = '';
      }
    }
    function remove(index) {
      todos.update(items => {
        const updatedItems = [...items];
        updatedItems.splice(index, 1);
        saveTodos();
        return updatedItems;
      });
    }
    function getTodosFromLocalStorage() {
      return JSON.parse(localStorage.getItem('todos'));
    }
    function clearTodos() {
      todos.set([]);
      saveTodos();
    }
    function markAllAsDone() {
      todos.update(items => {
        const updatedItems = items.map(todo => {
          return { ...todo, done: true };
        });
        saveTodos();
        return updatedItems;
      });
    }
  </script>
  <h1>TODO APP</h1>
  <!-- text field -->
  <div class="input-container">
    <input
      type="text"
      class="todo-input"
      placeholder="Enter a task..."
      bind:value={todoText}
      on:keydown={(event) => {
        if (event.key === 'Enter') {
          addTodo();
        }
      }}
    />
    <!-- add button -->
    <button on:click={addTodo} class="action-button">Add</button>
  </div>
  {#each $todos as todo, index}
    <!-- todo item -->
    <div class="todo-entry" class:done={todo.done}>
      <!-- text -->
      <div>{todo.text}</div>
      <!-- checkboxes -->
      <input
        type="checkbox"
        bind:checked={$todos[index].done}
        id={`checkbox-${index}`}
      />
      <label for={`checkbox-${index}`} class="checkbox-label">Done</label>
      <button class="delete" on:click={() => remove(index)}>Delete</button>
    </div>
  {/each}
  <div class="button-container">
    <button on:click={clearTodos} class="action-button">Clear All</button>
    <button on:click={markAllAsDone} class="action-button">Mark All as Done</button>
  </div>
  <style>
    .input-container {
      display: flex;
      align-items: center;
      margin-bottom: 16px;
    }
    .todo-input {
      flex: 1;
      width: 300px;
      height: 32px;
      border: 1px solid #ccc;
      border-radius: 4px;
      padding: 8px;
      margin-right: 8px;
      font-size: 14px;
    }
    .action-button {
      background-color: #4caf50;
      color: white;
      border: none;
      border-radius: 4px;
      padding: 8px 16px;
      margin-right: 8px;
      cursor: pointer;
      font-size: 14px;
      transition: background-color 0.3s ease;
    }
    .action-button:hover {
      background-color: #45a049;
    }
    .delete {
      background-color: #f44336;
      color: white;
      border: none;
      border-radius: 4px;
      padding: 8px 16px;
      margin-left: 8px;
      cursor: pointer;
      font-size: 14px;
      transition: background-color 0.3s ease;
    }
    .delete:hover {
      background-color: #d32f2f;
    }
    .checkbox-label {
      margin-left: 8px;
    }
    .done {
      color: #888;
      text-decoration: line-through;
    }
    .todo-entry {
      display: flex;
      align-items: center;
      margin-bottom: 8px;
    }
    .button-container {
      margin-top: 16px;
    }
  </style>