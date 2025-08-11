<script>
	import { createEventDispatcher } from "svelte";
	export let contentHeight;
	export let contentWidth;
	export let page;

	const dispatch = createEventDispatcher();

	/**
     * @param {number | undefined} [additionalPoint]
     * @param {number} [pageIncrement]
     */
	function handleClick(choice, pageIncrement) {
	   dispatch('nextPage', { additionalChoice: choice, pageIncrement });
   }

</script>

<div class="content" style="height: {contentHeight}px; width: {contentWidth}px;">

	<div class="space"></div>

	<!-- {#if page == 6}
		<div class="as">
			<button on:click={() => handleClick(0, 1)}>ไม่มีอะไรหรอก คงเป็นแค่เสียงลมที่กระทบกับหน้าต่าง</button>
			<button on:click={() => handleClick(0, 1)}>ใจเริ่มสั่น ตัวคุณเองก็กลัว และเริ่มจินตนาการไปต่าง ๆ นานา</button>
			<button on:click={() => handleClick(0, 2)}>คุณจะลุกขึ้นมาเปิดไฟ</button>
			<button on:click={() => handleClick(0, 2)}>เรามันแน่อยู่แล้ว ขอก้มลงไปดูใต้เตียงเลยแล้วกัน !</button>
		</div>
	{/if}
	{#if page == 12}
		<div class="as">
			<button on:click={() => handleClick(0, 1)}>คุณมึนงงปนตกใจ พร้อมรีบก้าวลงจากรถเข้าไปอุ้มแมวตัวนั้น</button>
			<button on:click={() => handleClick(0, 1)}>คุณตื่นเต้น พร้อมลุกจากรถไปอุ้มเจ้าเหมียวเดินทางไปด้วยกัน</button>
			<button on:click={() => handleClick(0, 2)}>คุณชะโงกหน้าออกไปดูลาดเลา เจ้าแมว เพื่อที่จะได้พิจารณามัน</button>
			<button on:click={() => handleClick(0, 2)}>คุณคิดว่ามันแปลกเกินไปที่แมวจะอยู่ที่นี่ และคงไม่ดีนักที่จะไปยุ่งกับมัน</button>
		</div>
	{/if} -->
	<!-- const page_n = [9,12,18,21,25,28]; -->
	{#if page == 9}
		<div class="as">
			<button on:click={() => handleClick("A", 1)}></button>
			<button on:click={() => handleClick("B", 1)}></button>
			<button on:click={() => handleClick("C", 1)}></button>
			<button on:click={() => handleClick("D", 1)}></button>
		</div>
	{/if}
	{#if page == 12}
		<div class="as">
			<button on:click={() => handleClick("A", 1)}></button>
			<button on:click={() => handleClick("B", 1)}></button>
			<button on:click={() => handleClick("C", 1)}></button>
			<button on:click={() => handleClick("D", 1)}></button>
		</div>
	{/if}
	{#if page == 18}
		<div class="as">
			<button on:click={() => handleClick("A", 1)}></button>
			<button on:click={() => handleClick("B", 1)}></button>
			<button on:click={() => handleClick("C", 1)}></button>
			<button on:click={() => handleClick("D", 1)}></button>
		</div>
	{/if}
	{#if page == 21}
		<div class="as">
			<button on:click={() => handleClick("A", 1)}></button>
			<button on:click={() => handleClick("B", 1)}></button>
			<button on:click={() => handleClick("C", 2)}></button>
			<button on:click={() => handleClick("D", 2)}></button>
		</div>
	{/if}
	{#if page == 25}
		<div class="as">
			<button on:click={() => handleClick("A", 1)}></button>
			<button on:click={() => handleClick("B", 1)}></button>
			<button on:click={() => handleClick("C", 1)}></button>
			<button on:click={() => handleClick("D", 1)}></button>
		</div>
	{/if}
	{#if page == 28}
		<div class="as">
			<button on:click={() => handleClick("A", 1)}></button>
			<button on:click={() => handleClick("B", 1)}></button>
			<button on:click={() => handleClick("C", 1)}></button>
			<button on:click={() => handleClick("D", 1)}></button>
		</div>
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
		grid-template-rows: 50% 34% 16%;
		width: 100%;
	}
	.as {
		height: 100%;
		width: 84%;
		display: grid;
		align-items: center;
		grid-template-rows: 1fr 1fr 1fr 1fr;
		gap: 5%;
	}
	.as button {
		font-family: 'CloudLoop';
		font-size: 14px;
		color: darkslategrey;
		height: 100%;
		width: 100%;
		border-radius: 200px;
		border: none;
		background-color: transparent;
		/* background-color: aliceblue; */
	}
	.space {
		height: 100%;

	}

</style>
