<script lang="ts">

	import LART from '../../public/Img/GO/LART.png';
	import CITU from '../../public/Img/GO/CITU.png';
	import LLB from '../../public/Img/GO/LLB.png';
	import BIR from '../../public/Img/GO/BIR.png';
	import PBIC from '../../public/Img/GO/PBIC.png';
	import SPD from '../../public/Img/GO/SPD.png';
	import TBS from '../../public/Img/GO/TBS.png';
	import BJM from '../../public/Img/GO/BJM.png';
	import BE from '../../public/Img/GO/BE.png';
	import PPE from '../../public/Img/GO/PPE.png';


	import { onMount } from "svelte";
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	export let point: number[];
	let totalPoints = 0;
	let gift = "";

	$: calculateTotalPoints();
	$: determineGift();

	function calculateTotalPoints() {
		totalPoints = point.reduce((acc, curr) => acc + curr, 0);
		console.log(totalPoints);
	}
	function determineGift() {
		// Ver jeng final
		if (totalPoints >= 1.592 && totalPoints <= 4.023) {
			gift = BIR;
		} else if (totalPoints > 4.023 && totalPoints <= 4.418) {
			gift = TBS;
		} else if (totalPoints > 4.418 && totalPoints <= 4.705) {
			gift = BE;
		} else if (totalPoints > 4.705 && totalPoints <= 4.954) {
			gift = PPE;
		} else if (totalPoints > 4.954 && totalPoints <= 5.186) {
			gift = SPD;
		} else if (totalPoints > 5.186 && totalPoints <= 5.420) {
			gift = BJM;
		} else if (totalPoints > 5.420 && totalPoints <= 5.668) {
			gift = CITU;
		} else if (totalPoints > 5.668 && totalPoints <= 5.958) {
			gift = PBIC;
		} else if (totalPoints >= 5.958 && totalPoints <= 6.355) {
			gift = LLB;
		// [4.99 , 8.882] // 4.99 - 6.00
		} else if (totalPoints >= 6.355) {
			gift = LART;
		}
	}
	// 	if (totalPoints >= 1.696 && totalPoints <= 2.4501) {
	// 		gift = BIR;
	// 	} else if (totalPoints > 2.4501 && totalPoints <= 3.2052) {
	// 		gift = TBS;
	// 	} else if (totalPoints > 3.2052 && totalPoints <= 3.9603) {
	// 		gift = BE;
	// 	} else if (totalPoints > 3.9603 && totalPoints <= 4.7154) {
	// 		gift = PPE;
	// 	} else if (totalPoints > 4.7154 && totalPoints <= 5.4705) {
	// 		gift = SPD;
	// 	} else if (totalPoints > 5.4705 && totalPoints <= 6.2256) {
	// 		gift = BJM;
	// 	} else if (totalPoints > 6.2256 && totalPoints <= 6.9807) {
	// 		gift = CITU;
	// 	} else if (totalPoints > 6.9807 && totalPoints <= 7.7358) {
	// 		gift = PBIC;
	// 	} else if (totalPoints >= 7.7358 && totalPoints <= 8.4919) {
	// 		gift = LLB;
	// 	// [4.99 , 8.882] // 4.99 - 6.00
	// 	} else if (totalPoints >= 8.4919) {
	// 		gift = LART;
	// 	}
	// }

	function reset() {
		window.location.reload();
	}

	function downloadImage(gift: string) {
		const link = document.createElement('a');
		link.href = gift;
		link.download = `gift.png`;
		document.body.appendChild(link);
		link.click();
		document.body.removeChild(link);
	}

	onMount(() => {
	});

</script>

<div class="container">

	<img src={gift} alt="{gift}" />

	<div class="controls">
		<button on:click={reset}>ReStart Game</button>
		<button on:click={() => downloadImage(gift)}>Download Image</button>
	</div>

</div>

<style>

	@font-face {
		font-family: 'CloudLoop';
		src: url('./font/CloudLoop-Regular.otf') format('opentype');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}
	.container {
		display: grid;
		grid-template-rows: 90% 10%;
		align-items: center;
		justify-items: center;
		height: 100%;
		width: auto;
		object-fit: contain;
	}

	img {
		max-width: 100%;
		height: auto;
	}

	.controls {
		display: grid;
		grid-template-columns: 1fr 1fr;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
		padding-top: 5%;
	}

	button {
		font-family: 'CloudLoop';
		font-size: 16px;
		min-width: 60%;
		padding: 5px;
		padding-left: 10%;
		padding-right: 10%;
		color: black;
		border: 1px solid black;
		border-radius: 20px;
		background-color: white;
		transition: background-color 0.3s;
		align-self: center;
		justify-self: center;
	}
</style>
