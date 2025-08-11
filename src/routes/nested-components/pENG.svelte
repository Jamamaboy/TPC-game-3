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

	const dic: Record<string, number> = { "A": 0, "B": 0, "C": 0, "D": 0 };

	function calculateTotalPoints() {
		totalPoints = point.reduce((acc, curr) => acc + (dic[curr] ?? 0), 0);
		console.log(totalPoints);
	}

	function determineGift() {
		// นับจำนวนแต่ละตัวเลือก
		const count: Record<string, number> = { A: 0, B: 0, C: 0, D: 0 };
		let lastChoice = "A";
		for (const ans of point) {
			if (count[ans] !== undefined) count[ans]++;
			if (["A","B","C","D"].includes(ans)) lastChoice = ans;
		}
		// หา key ที่มีค่ามากสุด
		let max = 0;
		let maxChoices: string[] = [];
		for (const k of Object.keys(count)) {
			if (count[k] > max) {
				max = count[k];
				maxChoices = [k];
			} else if (count[k] === max && max > 0) {
				maxChoices.push(k);
			}
		}
		// ถ้ามีมากสุดมากกว่า 1 ตัวเลือก ให้ใช้ตัวสุดท้ายที่ตอบ
		let finalChoice = maxChoices.length === 1 ? maxChoices[0] : lastChoice;
		if (finalChoice === "A") gift = Earth;
		else if (finalChoice === "B") gift = Fire;
		else if (finalChoice === "C") gift = Water;
		else if (finalChoice === "D") gift = Wind;
		else gift = "";
	}


	function reset() {
		window.location.reload();
	}

	function downloadImage(gift: string) {
		const link = document.createElement('a');
		link.href = gift;
		// หาชื่อไฟล์จาก gift ที่ตรงกับ element
		let fileName = 'gift.png';
		if (gift === Earth) fileName = 'earth pyrite#TPCFirstMeet2025.png';
		else if (gift === Fire) fileName = 'fire garnet#TPCFirstMeet2025.png';
		else if (gift === Water) fileName = 'water moonstone#TPCFirstMeet2025.png';
		else if (gift === Wind) fileName = 'wind lapis lazuli#TPCFirstMeet2025.png';
		link.download = fileName;
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
