<script>
	// export let name;
	let todos = JSON.parse(localStorage.getItem('todos')) || [];
	let newTodo = '';

	function addTodo() {
    if (newTodo.trim() !== '') {
        todos = [...todos, {id: Date.now(), text: newTodo, completed: false}];
        newTodo = '';
    }
}

	function toggleCompleted(id) {
	todos = todos.map(todo =>
		todo.id ===	id ? {...todo, completed: !todo.completed} : todo );
}
	function deleteTodo(id) {
		todos = todos.filter(todo => todo.id !== id);
	}

	$: localStorage.setItem('todos', JSON.stringify(todos));
</script>

<h1>Todo List</h1>

<div> 
	<input type="text" bind:value={newTodo} placeholder="Add a new todo..." />
	<button on:click={addTodo}>Add</button>
</div>

<ul>
    {#each todos as todo (todo.id)}
        <li class:completed={todo.completed}>
            <input type="checkbox" checked={todo.completed} on:change={() => toggleCompleted(todo.id)} />
            {todo.text}
			<button on:click={() => deleteTodo(todo.id)}>Delete</button>
        </li>
    {/each}
</ul>

<style>
	.completed {
		text-decoration: line-through;
		color:bisque;
		}
</style>