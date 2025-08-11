<script lang="ts">

	import Earth from '../../public/Img/GO/earth_pyrite.png';
	import Fire from '../../public/Img/GO/fire_garnet.png';
	import Water from '../../public/Img/GO/water_moonstone.png';
	import Wind from '../../public/Img/GO/wind_lapis.png';

	import { onMount } from "svelte";
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	export let point: string[];
	let totalPoints = 0;
	let gift = "";

	$: calculateTotalPoints();
	$: determineGift();

	const dic: Record<string, number> = { "A": 1, "B": 2, "C": 0, "D": 0 };

	function calculateTotalPoints() {
		totalPoints = point.reduce((acc, curr) => acc + (dic[curr] ?? 0), 0);
		console.log(totalPoints);
	}

	function determineGift() {

		// // Ver jeng final
		// if (totalPoints >= 1.592 && totalPoints <= 4.023) {
		// 	gift = BIR;
		// } else if (totalPoints > 4.023 && totalPoints <= 4.418) {
		// 	gift = TBS;
		// } else if (totalPoints > 4.418 && totalPoints <= 4.705) {
		// 	gift = BE;
		// } else if (totalPoints > 4.705 && totalPoints <= 4.954) {
		// 	gift = PPE;
		// } else if (totalPoints > 4.954 && totalPoints <= 5.186) {
		// 	gift = SPD;
		// } else if (totalPoints > 5.186 && totalPoints <= 5.420) {
		// 	gift = BJM;
		// } else if (totalPoints > 5.420 && totalPoints <= 5.668) {
		// 	gift = CITU;
		// } else if (totalPoints > 5.668 && totalPoints <= 5.958) {
		// 	gift = PBIC;
		// } else if (totalPoints >= 5.958 && totalPoints <= 6.355) {
		// 	gift = LLB;
		// // [4.99 , 8.882] // 4.99 - 6.00
		// } else if (totalPoints >= 6.355) {
		// 	gift = LART;
		// }
	}


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
