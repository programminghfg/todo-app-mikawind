<script>
    let todoText = '';
    let todos = [];

    function addToDo() {
        todos.push({ text: todoText, done: false });
        todos = todos;
        todoText = '';
    }

    function remove(index) {
        todos.splice(index, 1);
        todos = todos;
    }
</script>

<!--textfeld-->
<input type="text" class="todo-input" bind:value={todoText} />
<button class="Add"on:click={addToDo}>ADD</button>
<!-- button add -->

<!-- todo -->
{#each todos as todo, index}
    <div class="todo-entry" class:done={todo.done}>
        <div>{todo.text}</div>
        <input type="checkbox" bind:value={todo.done}/>
        <button class="delete"on:click={(index) => {remove(index); }}>X</button>
    </div>
{/each}
<!-- text -->
<!-- checkboxen -->
<style>
    .done {
        color: rgb(135, 135, 143);
    }
    .todo-entry{
        display: flex;
    }
    .todo-input{
        color: rgb(13, 13, 214);
    }
    .Add{
        color: rgb(0, 139, 21);
        font-size: medium;
    }
</style>