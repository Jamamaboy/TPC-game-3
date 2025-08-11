<script lang="ts">
	import { dev } from '$app/environment';
	import { inject } from '@vercel/analytics';

	inject({ mode: dev ? 'development' : 'production' });

	let page = 0;
	let point: string[] = ["0"];
	let contentHeight = 0;
	let contentWidth = 0;

	let history = [1];

	import { error } from '@sveltejs/kit';

	import Pzero from './nested-components/pzero.svelte';
	import PStart from './nested-components/pStart.svelte';
	import Ptbc from './nested-components/ptbc.svelte';
	import Pn from './nested-components/pn.svelte';
	import PP from './nested-components/pP.svelte';
	import PEng from './nested-components/pENG.svelte';

	const images: { [key: string]: { default: string } } = import.meta.glob('../public/Img/*.webp', { eager: true });
	const imageUrls: { [key: string]: string } = Object.keys(images).reduce((acc: { [key: string]: string }, path) => {
		const fileName = path.split('/').pop()?.replace('.webp', '');
		if (fileName) {
			acc[fileName] = images[path].default;
		}
		return acc;
	}, {});

	const page_zero = [0];
	const page_Start = [1];
	const page_tbc = [2, 3, 4, 5, 6, 7, 8, 10, 11, 13, 14, 15, 16, 17, 19, 20, 22, 23, 24, 26, 27, 29, 30, 31, 32, 33];
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
		// ถ้าอยู่หน้า 0 แล้วไปหน้าใหม่ ให้ไป 2
		if (page === 0) {
			nextPage = 2;
		} else {
			nextPage = page + pageIncrement;
		}
		history.push(page);
		page = nextPage;
	}

	function handlePreviousPage() {
		if (history.length > 0) {
			const previousPage = history.pop();
			if (previousPage !== undefined) {
				// If moving back from page 2, go to page 1 instead of 0
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

	function getImageSrc(page: number) {
		// console.log(imageUrls);
		return imageUrls[`${page}`] || '';
	}
</script>

<body>

	<!-- const page_zero = [0];
	const page_Start = [1];
	const page_tbc = [2, 3, 4, 5, 6, 7, 8, 10, 11, 13, 14, 15, 16, 17, 19, 20, 22, 23, 24, 26, 27, 29, 30, 31, 32, 33];
	const page_n = [9,12,18,21,25,28];
	const page_nextP = [34];
	const page_ENG = [35]; -->
	<!-- Zero [0] -->
	{#if page_zero.includes(page)}
		<img src={getImageSrc(page + 1)} alt="P{page + 1}" on:load={handleImageLoad}>
		<Pzero contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
	{/if}

	<!-- Start [1]-->
	{#if page_Start.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<PStart contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
	{/if}

	<!-- TBC [2, 3, 4, 5, 6, 7, 8, 10, 11, 13, 14, 15, 16, 17, 19, 20, 22, 23, 24, 26, 27, 29, 30, 31, 32, 33] -->
	{#if page_tbc.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<Ptbc contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} on:previousPage={handlePreviousPage} />
	{/if}

	<!-- N [6,12,18,21,25,28] -->
	{#if page_n.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<Pn contentHeight={contentHeight} contentWidth={contentWidth} page={page} on:nextPage={handleNextPage} />
	{/if}


	<!-- NextP [34] -->
	{#if page_nextP.includes(page)}
		<img src={getImageSrc(page)} alt="P{page}" on:load={handleImageLoad}>
		<PP contentHeight={contentHeight} contentWidth={contentWidth} on:nextPage={handleNextPage} />
	{/if}

	<!-- ENG [35] -->
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
		min-height: 100dvh;
		margin: 0;
	}
	img {
		position: absolute;
		z-index: 0;
		max-width: 100dvw;
		max-height: 100dvh;
		width: auto;
		height: auto;
		object-fit: contain;
	}

</style>
