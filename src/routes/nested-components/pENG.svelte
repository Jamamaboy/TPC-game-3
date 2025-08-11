<script lang="ts">

	import Earth from '../../public/Img/GO/earth_pyrite.png';
	import Fire from '../../public/Img/GO/fire_garnet.png';
	import Water from '../../public/Img/GO/water_moonstone.png';
	import Wind from '../../public/Img/GO/wind_lapis.png';


	import { onMount } from "svelte";
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	export let point: string[];
	export let contentHeight: number;
	export let contentWidth: number;
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
<div class="content" style="height: {contentHeight}px; width: {contentWidth}px;">
	<div class="container">

		<img src={gift} alt="{gift}" />

		<div class="controls">
			<button on:click={reset}>Restart Game</button>
			<button on:click={() => downloadImage(gift)}>Download Image</button>
		</div>

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
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		padding: 10px;
	}

	img {
		width: 100%;
		max-width: 500px; /* กำหนดขนาดสูงสุดสำหรับจอใหญ่ */
		height: auto;
		object-fit: contain;
		flex: 1;
	}

	.controls {
		display: flex;
		gap: 20px;
		justify-content: center;
		align-items: center;
		margin-top: 10px;
		flex-shrink: 0;
	}

	@media (max-width: 768px) {
		img {
			max-width: 100%;
			max-height: 70vh; /* บนมือถือ ให้ภาพสูงสุดไม่เกิน 70% ของหน้าจอ */
		}
	}

	button {
		font-family: 'CloudLoop';
		font-size: 18px;
		padding: 10px 20px;
		color: white;
		background: linear-gradient(135deg, #f9c1ff, #a1c4fd); /* ไล่สีหวาน ๆ */
		border: none;
		border-radius: 30px;
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
		cursor: pointer;
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
</style>
