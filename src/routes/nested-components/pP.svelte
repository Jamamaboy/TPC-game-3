<script lang="ts">
	import { createEventDispatcher } from "svelte";

	export let contentHeight: number;
	export let contentWidth: number;

	const dispatch = createEventDispatcher();

	let showButton = false;

	function showButtonAfterDelay() {
		setTimeout(() => {
			showButton = true;
		}, 2500);
	}

	showButtonAfterDelay();

	/**
	 * @param {number} [pageIncrement]
	 */
	function handleClick(pageIncrement: number) {
		dispatch('nextPage', {pageIncrement});
	}

</script>

<div class="content" style="height: {contentHeight}px; width: {contentWidth}px;">
	<div class="space"></div>
	{#if showButton}
		<button on:click={() => handleClick(1)}>Hope it shines bright for you! 💎✨</button>
	{/if}
	<div class="space"></div>
</div>

<style>
	@font-face {
		font-family: 'CloudLoop';
		src: url('./font/CloudLoop-Regular.otf') format('opentype');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}
	.content {
		display: grid;
		z-index: 1;
		justify-items: center;
		align-items: center;
		grid-template-rows: 69% 39% 10%;
		width: 100%;
	}
	button {
		font-family: 'CloudLoop';
		cursor: pointer;
		outline: none;
		font-size: 18px;
		padding: 10px 20px;
		color: white;
		background: linear-gradient(135deg, #f9c1ff, #a1c4fd); /* ไล่สีพาสเทล */
		border: none;
		border-radius: 30px;
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
		transition: transform 0.2s ease, box-shadow 0.2s ease;
	}

	button:hover {
		transform: translateY(-3px);
		box-shadow: 0 6px 12px rgba(0, 0, 0, 0.25);
	}

	button:active {
		transform: translateY(0);
		box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
	}

	.space {
		height: 100%;
	}
</style>
