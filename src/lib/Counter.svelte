<script>
	import { init } from 'svelte/internal';

	export let initialCount = 0;
	export let maxCount;

	console.log($$props, $$restProps);

	let counter = initialCount;
	let message = '';

	$: message = `Hey, You clicked the counter ${counter} times.`;

	$: if (counter > maxCount) {
		message = `That's quite enough:  ${counter} times.`;
	}
</script>

<button on:click={() => (counter += 1)}> Click </button>
<p>{message}</p>

<!-- <h3>You have clicked {counter} times</h3> -->
<style>
	button {
		padding: 12px;
	}
</style>
