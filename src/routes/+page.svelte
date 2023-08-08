<script>
	// @ts-nocheck

	import typeScript from 'programming-languages-logos/src/typescript/typescript_128x128.png';
	import cLang from 'programming-languages-logos/src/c/c_128x128.png';
	import cpp from 'programming-languages-logos/src/cpp/cpp_128x128.png';
	import cSharp from 'programming-languages-logos/src/csharp/csharp_128x128.png';
	import java from 'programming-languages-logos/src/java/java_128x128.png';
	import javascript from 'programming-languages-logos/src/javascript/javascript_128x128.png';

	let tier1 = {
		title: 'A',
		content: []
	};
	let tier2 = {
		title: 'B',
		content: []
	};
	let tier3 = {
		title: 'C',
		content: []
	};
	let tier4 = {
		title: 'D',
		content: []
	};
	let tier5 = {
		title: 'E',
		content: []
	};

	let poolItems = [
		{
			tier: null,
			src: javascript
		},
		{
			tier: null,
			src: java
		},
		{
			tier: null,
			src: cLang
		},
		{
			tier: null,
			src: cSharp
		},
		{
			tier: null,
			src: cpp
		},
		{
			tier: null,
			src: typeScript
		}
	];

	let tiers = [tier1, tier2, tier3, tier4, tier5];

	let currentItem = null;

	function dragHandler(e, item) {
		e.preventDefault();
		const { left, right, top, bottom } = e.target.getBoundingClientRect();
		e.left = left;
		e.top = top;

		currentItem = e;
		e.target.style.transition = 'box-shadow, opacity 0.5s';
		e.target.style.opacity = '0.5';
		e.target.style.position = 'absolute';
		e.target.style.left = currentX - (e.pageX - left) + 'px';
		e.target.style.top = currentY - (e.pageY - top) + 'px';
		e.target.style.boxShadow = '0px 0px 20px 10px black';
		e.target.style.zIndex = '1000';
	}

	// @ts-ignore
	function dragStopHandler(e, item) {
		// currentItem = null;
	}

	let isMouseDown = false;
	let currentX = 0;
	let currentY = 0;
	let currentTierHover = null;

	function mouseMoveHandler(e) {
		currentX = e.pageX;
		currentY = e.pageY;
		if (isMouseDown && currentItem) {
			console.log(currentItem.pageX - currentItem.left);
			let diff = currentItem.pageX - currentItem.left;
			currentItem.target.style.left = currentX - diff + 'px';
			currentItem.target.style.top = currentY - (currentItem.pageY - currentItem.top) + 'px';
			const behindElements = document.elementsFromPoint(e.clientX, e.clientY);
			currentTierHover = behindElements.find((i) => i.classList.contains('tier'));
			if (currentTierHover) {
				console.log(currentTierHover);
				currentTierHover = currentTierHover.lastElementChild;
			}
		}
	}
	function mouseDownHandler() {
		isMouseDown = true;
	}

	function mouseUpHandler() {
		console.log(currentItem);
		isMouseDown = false;
		if (currentTierHover) {
			console.log('above tier', currentTierHover);
			currentTierHover.append(currentItem.target);
			currentItem.target.onmousedown = dragHandler;
			currentItem.target.onmouseup = dragStopHandler;
		}
		if (currentItem) {
			currentItem.target.style.position = 'relative';
			currentItem.target.style.left = 'unset';
			currentItem.target.style.top = 'unset';
			currentItem.target.style.boxShadow = 'unset';
			currentItem.target.style.zIndex = 'unset';
			currentItem.target.style.opacity = 'unset';
			currentItem = null;
		}
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<section
	on:mousemove={mouseMoveHandler}
	on:mousedown={mouseDownHandler}
	on:mouseup={mouseUpHandler}
>
	<div class="container">
		{#each tiers as tier}
			<div class="tier" id={tier.title}>
				<div class="tier-grade">{tier.title}</div>
				<div class="tier-solt" />
			</div>
		{/each}
	</div>
	<div class="pool">
		{#each poolItems as item}
			<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<div
				class="pool-item"
				on:mousedown={(e) => {
					dragHandler(e, item);
				}}
				on:mouseup={(e) => {
					dragStopHandler(e, item);
				}}
				on:click={() => console.log('HERE')}
			>
				<img src={item.src} height="128" width="128" alt="Langauge" />
			</div>
		{/each}
	</div>
</section>

<style>
	.container {
		display: flex;
		flex-direction: column;
		gap: 1px;
	}
	.tier {
		display: flex;
		min-height: 120px;
		background-color: black;
	}
	.tier-solt {
		display: flex;
		padding: 1em;
		align-items: center;
		gap: 1em;
		transition: all 0.5s;
	}
	.tier-grade {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 120px;
		background-color: rgb(255, 166, 0);
	}
	.pool {
		display: flex;
		gap: 1em;
	}

	.pool-item {
		transition: all 1s;
	}
</style>
