<script>
	import Button from './lib/Button.svelte';
	import FaAngellist from 'svelte-icons/fa/FaAngellist.svelte';
	import FaAllergies from 'svelte-icons/fa/FaAllergies.svelte';
</script>

<Button let:isLeftHovered size="large">
	<div style="width:20px;" slot="leftContent">
		{#if isLeftHovered}
			<FaAngellist />
		{:else}
			<FaAllergies />
		{/if}
	</div>
	Hello from the other side
</Button>
<Button size="large">Hello from the other side</Button>
<Button size="small" bgColor="blue" color="">Hello from this side</Button>

<!-- <script>
	import Counter from './lib/Counter.svelte';

	const props = {
		initialCount: 0,
		maxCount: 5
	};
</script>

<Counter initialCount={3} maxCount={10} />
<Counter {...props} somthingIdontNeed="999" /> -->
