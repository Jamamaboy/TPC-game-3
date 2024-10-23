<script lang="ts">
	import { dev } from '$app/environment';
	import { inject } from '@vercel/analytics';

	inject({ mode: dev ? 'development' : 'production' });
	let page = 1;
	let point = [0];
	let contentHeight = 0;
	let contentWidth = 0;

	let history = [1];

	import PStart from './nested-components/pStart.svelte';
	import Ptbc from './nested-components/ptbc.svelte';
	import Pn from './nested-components/pn.svelte';
	import Ps from './nested-components/ps.svelte';
	import Pl from './nested-components/pl.svelte';
	import PEng from './nested-components/pENG.svelte';
	import PP from './nested-components/pP.svelte';
	import { error } from '@sveltejs/kit';

	const images: { [key: string]: { default: string } } = import.meta.glob('../public/Img/*.png', { eager: true });
	const imageUrls: { [key: string]: string } = Object.keys(images).reduce((acc: { [key: string]: string }, path) => {
		const fileName = path.split('/').pop()?.replace('.png', '');
		if (fileName) {
			acc[fileName] = images[path].default;
		}
		return acc;
	}, {});

	const page_Start = [1];
	const page_tbc = [2, 3, 7, 8, 13, 14, 16, 17, 18, 20, 22, 24, 26, 31, 34, 36, 37, 38];
	const page_nextX2 = [4, 5];
	const page_nextX3 = [9, 10, 11];
	const page_n = [6, 12];
	const page_s = [15];
	const page_l = [19, 21, 23, 25, 27, 28, 29, 30, 32, 33, 35];
	const page_nextP = [39];
	const page_ENG = [40];

	function handleImageLoad(event: { target: any; }) {
		const img = event.target;
		contentHeight = img.offsetHeight;
		contentWidth = img.offsetWidth;
	}

	function handleNextPage(event: { detail: { additionalPoint: any; pageIncrement: any; }; }) {
		const { additionalPoint, pageIncrement } = event.detail;

		if (additionalPoint > 0) {
			point.push(additionalPoint);
		}

		let nextPage;
		switch (page) {
			case 7:
				nextPage = 9;
				break;
			case 13:
				nextPage = 15;
				break;
			case 36:
				nextPage = 38;
				break;
			default:
				nextPage = page + pageIncrement;
		}
		history.push(page);
		page = nextPage;
	}

	function handlePreviousPage() {
		if (history.length > 0) {
			const previousPage = history.pop();
			if (previousPage !== undefined) {
				page = previousPage;
			}
		} else {
			alert('No previous pages in history.');
		}
	}

	function getImageSrc(page: number) {
		// console.log(imageUrls);
		return imageUrls[`${page}`] || '';
	}
</script>

<body>
	<!-- Start [1]-->
	{#if page_Start.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<PStart contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
	{/if}

	<!-- TBC [2,3,7,8,13,14,16,17,18,20,24,26,31,34,36,37,38], NextX2 [4,5], NextX3 [9,10,11] -->
	{#if page_tbc.includes(page) || page_nextX2.includes(page) || page_nextX3.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<Ptbc contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} on:previousPage={handlePreviousPage} />
	{/if}

	<!-- N [6,12] -->
	{#if page_n.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<Pn contentHeight={contentHeight} contentWidth={contentWidth} page={page} on:nextPage={handleNextPage} />
	{/if}

	<!-- S [15] -->
	{#if page_s.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<Ps contentHeight={contentHeight} contentWidth={contentWidth} page={page} on:nextPage={handleNextPage} />
	{/if}

	<!-- L [19,21,23,25,27,28,29,30,32,33,35] -->
	{#if page_l.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<Pl contentHeight={contentHeight} contentWidth={contentWidth} page={page} on:nextPage={handleNextPage} />
	{/if}

	<!-- NextP [39] -->
	{#if page_nextP.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<PP contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
	{/if}

	<!-- ENG [40] -->
	{#if page_ENG.includes(page)}
		<PEng point={point} />
	{/if}
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
		min-height: 100vh;
		margin: 0;
	}

	img {
		position: absolute;
		z-index: 0;
		max-width: 100vw;
		max-height: 100vh;
		width: auto;
		height: auto;
		object-fit: contain;
	}
</style>
