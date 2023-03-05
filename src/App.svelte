<svelte:options immutable={true} />

<script>
	import { v4 as uuid } from 'uuid';
	import Button from './lib/Button.svelte';
	import ToDoList from './lib/ToDoList.svelte';
	let showTodoList = true;
	let todos = [
		{
			id: uuid(),
			title: 'Wake up',
			completed: true
		},
		{
			id: uuid(),
			title: 'Eat',
			completed: false
		},
		{
			id: uuid(),
			title: 'Play',
			completed: false
		},
		{
			id: uuid(),
			title: 'Sleep',
			completed: true
		},
		{
			id: uuid(),
			title:
				'Lorem ipsum dolor sit amet consectetur adipisicing elit. Necessitatibus voluptate neque numquam repellat nulla eius! Doloremque, error consequatur voluptate quidem exercitationem incidunt placeat, porro nostrum quas, culpa omnis veniam.',
			completed: false
		}
	];
	let todoList;

	function loadTodos() {
		return fetch('https://jsonplaceholder.typicode.com/todos?_limit=10').then((response) => {
			if (response.ok) {
				return response.json();
			} else {
				throw new Error("Couldn't load todos from API");
			}
		});
	}

	function handleAddTodo(event) {
		todos = [...todos, { id: uuid(), title: event.detail.title, completed: false }];
		// todoList.clearInput();
	}

	function handleRemoveTodo(event) {
		todos = todos.filter((x) => x.id != event.detail.id);
	}

	function handleToggleTodo(event) {
		todos = todos.map((x) => {
			if (x.id === event.detail.id) {
				return { ...x, completed: event.detail.value };
			}
			return { ...x };
		});
	}
	// $: console.log(todos);
</script>

<input type="checkbox" bind:checked={showTodoList} /> Show todo list

{#if showTodoList}
	{#await loadTodos()}
		<p>Please wait while we load the todos</p>
	{:then todos}
		<ToDoList
			{todos}
			bind:this={todoList}
			on:addTodo={handleAddTodo}
			on:removeTodo={handleRemoveTodo}
			on:toggleTodo={handleToggleTodo}
		/>
		<!-- <button on:click={() => todoList.focusInput()}> Focus input </button> -->
	{:catch error}
		<p>{error.message || 'An error has occrured'}</p>
	{/await}
{/if}
