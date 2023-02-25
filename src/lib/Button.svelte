<script>
	export let size = 'small';
	export let shadow = true;
	export let bgColor = 'inherit';
	export let color = 'inherit';

	let isLeftHovered;
</script>

<!-- <button class={size === 'large' ? 'size-lg' : 'size-sm'}><slot /></button> -->
<button
	style:--buttonBgColor={bgColor}
	style:--buttonColor={color}
	class:size-lg={size === 'large'}
	class:size-sm={size != 'large'}
	class:shadow
>
	<div
		class="left-content"
		on:mouseenter={() => (isLeftHovered = true)}
		on:mouseleave={() => (isLeftHovered = false)}
	>
		{#if $$slots.leftContent}
			<slot name="leftContent" />
		{/if}
	</div>
	<slot {isLeftHovered} />
</button>

<style lang="scss">
	// @use './styles/variables.scss';
	// background-color: variables.$svelteColor;

	button {
		background-color: var(--buttonBgColor);
		color: var(--buttonColor);
		display: flex;
		align-items: center;
		border: none;
		font-weight: bold;
		cursor: pointer;
		border-radius: 5px;
		&:hover {
			background-color: blueviolet;
		}
		&.size-sm {
			padding: 15px 20px;
		}
		&.size-lg {
			padding: 20px 25px;
			font-size: 20px;
		}
		&.shadow {
			box-shadow: 0 0 10px rgba(255, 255, 255, 0.6);
		}
		.left-content {
			margin-right: 10px;
		}
	}
</style>
