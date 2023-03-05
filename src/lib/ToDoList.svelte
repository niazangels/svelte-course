<script>
	import Button from './Button.svelte';
	import { createEventDispatcher, onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte';

	import FaRegTrashAlt from 'svelte-icons/fa/FaRegTrashAlt.svelte';

	const dispatch = createEventDispatcher();
	export let todos = [];
	export function clearInput() {
		inputText = '';
	}

	export function focusInput() {
		input.focus();
	}

	let input, listDiv, shouldAutoscroll;
	let prev_todos = todos;

	let inputText = '';

	// Keep track of whether we need to scroll

	$: {
		shouldAutoscroll = prev_todos.length != todos.length;
		prev_todos = todos;
	}

	// Lifecycle methods

	// onMount(() => {
	// 	console.log('Mounted');
	// });

	// onDestroy(() => {
	// 	console.log('Destroyed');
	// });

	// beforeUpdate(() => {
	// if (listDiv) {
	// console.log(`Height before: ${listDiv.offsetHeight}`);
	// }
	// });

	afterUpdate(() => {
		if (listDiv && shouldAutoscroll) {
			listDiv.scrollTo(0, listDiv.scrollHeight);
			shouldAutoscroll = false;
		}
	});

	function handleAddTodo() {
		if (!inputText) return;
		dispatch('addTodo', { title: inputText }, { cancelable: true });
	}

	function handleRemoveTodo(id) {
		dispatch('removeTodo', { id: id });
	}

	function handleToggleTodo(id, value) {
		dispatch('toggleTodo', { id: id, value: value });
	}
</script>

<div class="todo-list-wrapper">
	{#if !todos.length}
		<p class="no-items-text">No todos yet!</p>
	{:else}
		<ul class="todo-list" bind:this={listDiv}>
			{#each todos as { id, title, completed }, index (id)}
				<!-- {@debug id, title} -->
				<li class:completed>
					<label>
						<input
							type="checkbox"
							checked={completed}
							on:input={(event) => {
								event.currentTarget.checked = completed;
								handleToggleTodo(id, !completed);
							}}
						/>
						{title}
					</label>
					<button
						aria-label="Remove todo: {title}"
						on:click={() => handleRemoveTodo(id)}
						class="remove-todo-button"
					>
						<span style:display="inline-block" style:max-width="1rem">
							<FaRegTrashAlt />
						</span>
					</button>
				</li>
			{/each}
		</ul>
	{/if}
	<form action="#" class="add-todo-form" on:submit={handleAddTodo}>
		<input type="text" bind:this={input} bind:value={inputText} placeholder="Start typing" />
		<Button type="submit" disabled={!inputText}>Add</Button>
	</form>
</div>

<style lang="scss">
	.todo-list-wrapper {
		background-color: #424242;
		border: 1px solid #4b4b4b;
		max-width: 350px;
		.no-items-text {
			margin: 0;
			padding: 15px;
			text-align: center;
		}
		.add-todo-form {
			padding: 15px;
			background-color: #303030;
			display: flex;
			flex-wrap: wrap;
			border-top: 1px solid #4b4b4b;
			input {
				flex: 1;
				background-color: #424242;
				border: 1px solid #4b4b4b;
				padding: 10px;
				color: #fff;
				border-radius: 5px;
				margin-right: 10px;
			}
		}
	}
	.todo-list {
		max-height: 200px;
		overflow: auto;
		margin: 0;
		padding: 10px;
		list-style: none;
		li {
			margin-bottom: 5px;
			background-color: #303030;
			display: flex;
			align-items: center;
			border-radius: 5px;
			padding: 10px;
			position: relative;
			label {
				cursor: pointer;
				font-size: 18px;
				display: flex;
				align-items: baseline;
				input[type='checkbox'] {
					margin: 0 10px 0 0;
					cursor: pointer;
				}
				padding-right: 20px;
			}
			&.completed > label {
				opacity: 0.5;
				text-decoration: line-through;
			}
			.remove-todo-button {
				align-items: baseline;
				border: none;
				background: none;
				padding: 5px;
				position: absolute;
				right: 10px;
				top: 10px;
				cursor: pointer;
				display: none;
				:global(svg) {
					fill: #bd1414;
				}
			}
			&:hover {
				.remove-todo-button {
					display: block;
				}
			}
		}
	}
</style>
