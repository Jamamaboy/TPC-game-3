<script lang="ts">
	import { fly, fade } from 'svelte/transition';
	import { onMount } from 'svelte';
	// Preload all images in layer0 and layer1 for faster navigation
	onMount(() => {
		const totalPages = 34; // Adjust if you have more/less images
		for (let i = 0; i <= totalPages; i++) {
			const img0 = new window.Image();
			img0.src = `/Img/layer0/${i}.webp`;
			const img1 = new window.Image();
			img1.src = `/Img/layer1/${i}.webp`;
		}
	});
	import Pzero from './nested-components/pzero.svelte';
	import PStart from './nested-components/pStart.svelte';
	import Ptbc from './nested-components/ptbc.svelte';
	import Pn from './nested-components/pn.svelte';
	import PP from './nested-components/pP.svelte';
	import PEng from './nested-components/pENG.svelte';

	let page = 0;
	let point: string[] = ["0"];
	let contentHeight = 0;
	let contentWidth = 0;
	let history = [1];
	let prevPage = 0;
	let direction = 1; // 1 = next, -1 = prev

	function getLayer0Src(page: number) {
		return `/Img/layer0/${page}.webp`;
	}

	function getLayer1Src(page: number) {
		return `/Img/layer1/${page}.webp`;
	}

	const page_zero = [0];
	const page_Start = [1];
	const page_tbc = [2, 3, 4, 5, 6, 7, 8, 10, 11, 13, 14, 15, 16, 17, 19, 20, 22, 23, 24, 26, 27, 29, 30, 31, 32, 33];
	const page_cc = [3, 6];
	const page_n = [9,12,18,21,25,28];
	const page_nextP = [34];
	const page_ENG = [35];

	function handleImageLoad(event: { target: any; }) {
		const img = event.target;
		contentHeight = img.offsetHeight;
		contentWidth = img.offsetWidth;
	}

	function handleNextPage(event: { detail: { additionalChoice: string; pageIncrement: number; }; }) {
		const { additionalChoice, pageIncrement } = event.detail;
		if (additionalChoice && additionalChoice !== "0") {
			point.push(additionalChoice);
		} else {
			point.push("0");
		}
		let nextPage;
		if (page === 0) {
			nextPage = 2;
		} else {
			nextPage = page + pageIncrement;
		}
		direction = nextPage > page ? 1 : -1;
		prevPage = page;
		history.push(page);
		page = nextPage;
	}

	function handlePreviousPage() {
		if (history.length > 0) {
			const previousPage = history.pop();
			if (previousPage !== undefined) {
				direction = previousPage < page ? -1 : 1;
				prevPage = page;
				if (page === 2 && previousPage === 0) {
					page = 1;
				} else {
					page = previousPage;
				}
			}
		} else {
			alert('No previous pages in history.');
		}
	}
</script>
<body>
<main style="min-height: 100vh; min-width: 100vw; position: relative; overflow: hidden; background: #000;">
	<!-- Layer0: พื้นหลัง -->
	{#key page}
		<img
			src={getLayer0Src(page)}
			alt="bg"
			class="layer0-bg"
			in:fade={{ duration: 400 }}
			out:fade={{ duration: 400 }}
		/>
	{/key}

	<!-- Layer1: ภาพหลัก -->
	{#key page}
		<img
			src={getLayer1Src(page)}
			alt="main"
			class="layer1-img"
			on:load={handleImageLoad}
			in:fly={{ x: direction === 1 ? 100 : -100, duration: 400 }}
			out:fade={{ duration: 350 }}
		/>
	{/key}

	<!-- Foreground Content -->
	<div class="content-foreground">
		{#if page_zero.includes(page)}
			<Pzero contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
		{/if}
		{#if page_Start.includes(page)}
			<PStart contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
		{/if}
		{#if page_tbc.includes(page)}
			<Ptbc contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} on:previousPage={handlePreviousPage} />
		{/if}
		{#if page_n.includes(page)}
			<Pn contentHeight={contentHeight} contentWidth={contentWidth} page={page} on:nextPage={handleNextPage} />
		{/if}
		{#if page_nextP.includes(page)}
			<PP contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
		{/if}
		{#if page_ENG.includes(page)}
			<PEng contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} point={point} />
		{/if}
	</div>
</main>
</body>

<style>

	@font-face {
		font-family: 'CloudLoop';
		src: url('./font/CloudLoop-Regular.otf') format('opentype');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}
	body {
		display: flex;
		justify-content: center;
		align-items: center;
		min-height: 100dvh;
		margin: 0;
	}
	.layer0-bg {
		position: absolute;
		inset: 0;
		z-index: 0;
		width: 100vw;
		height: 100vh;
		object-fit: cover;
		pointer-events: none;
	}
	.layer1-img {
		position: absolute;
		inset: 0;
		z-index: 1;
		width: 100vw;
		height: 100vh;
		object-fit: contain;
		pointer-events: none;
		transition: filter 0.3s;
	}
	.content-foreground {
		position: absolute;
		z-index: 10;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		display: flex;
		justify-content: center;
		align-items: center;
		pointer-events: auto;
	}

</style>
