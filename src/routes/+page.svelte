<script>
	// @ts-ignore
	import { scaleLinear } from 'd3-scale';

	import Hello from '$lib/components/Hello.svelte';
    import Navbar from '$lib/components/Navbar.svelte';
    let name = $state('');
    let count = $state(0);
    function handleClick() {
        count += 1;
    }

	const points = [
		{ year: 1990, birthrate: 16.7 },
		{ year: 1995, birthrate: 14.6 },
		{ year: 2000, birthrate: 14.4 },
		{ year: 2005, birthrate: 14 },
		{ year: 2010, birthrate: 13 },
		{ year: 2015, birthrate: 12.4 }
	];

	const xTicks = [1990, 1995, 2000, 2005, 2010, 2015];
	const yTicks = [0, 5, 10, 15, 20];
	const padding = { top: 20, right: 15, bottom: 20, left: 25 };

	let width = $state(500);
	let height = $state(200);

	let user_birthrate = $state('');
	/**
     * @param {number} tick
     */
	function formatMobile(tick) {
		return "'" + tick.toString().slice(-2);
	}

	let xScale = $derived(scaleLinear()
		.domain([0, xTicks.length])
		.range([padding.left, width - padding.right]));

	let yScale = $derived(scaleLinear()
		.domain([0, Math.max.apply(null, yTicks)])
		.range([height - padding.bottom, padding.top]));

	let innerWidth = $derived(width - (padding.left + padding.right));
	let barWidth = $derived(innerWidth / xTicks.length);
</script>

<h2>US birthrate by year</h2>

<div class="chart" bind:clientWidth={width} bind:clientHeight={height}>
	<svg>
		<!-- y axis -->
		<g class="axis y-axis">
			{#each yTicks as tick}
				<g class="tick tick-{tick}" transform="translate(0, {yScale(tick)})">
					<line x2="100%" />
					<text y="-4">{tick} {tick === 20 ? ' per 1,000 population' : ''}</text>
				</g>
			{/each}
		</g>

		<!-- x axis -->
		<g class="axis x-axis">
			{#each points as point, i}
				<g class="tick" transform="translate({xScale(i)},{height})">
					<text x={barWidth / 2} y="-4">{width > 380 ? point.year : formatMobile(point.year)}</text>
				</g>
			{/each}
		</g>

		<g class="bars">
			{#each points as point, i}
				{#if point.year == 2000}
					<rect
						x={xScale(i) + 2}
						y={yScale(user_birthrate)}
						width={barWidth - 4}
						height={yScale(0) - yScale(user_birthrate)}
					/>
				{:else}
					<rect
						x={xScale(i) + 2}
						y={yScale(point.birthrate)}
						width={barWidth - 4}
						height={yScale(0) - yScale(point.birthrate)}
					/>
				{/if}
			{/each}
		</g>
	</svg>
</div>
<input bind:value={user_birthrate} type="number" placeholder="Enter the 2000's birthrate"/>

<!-- <input bind:value={name} placeholder="Enter your name" />
<button onclick={handleClick}>
    Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
{#if name != '' && count < 10}
    <h1 class="text-4xl text-center text-blue-500">Hello {name}!</h1>
{:else if count >= 10}
    <p>You are annoying</p> 
{/if} -->

<style>
	h2 {
		text-align: center;
	}

	.chart {
		width: 100%;
		max-width: 500px;
		margin: 0 auto;
	}

	svg {
		position: relative;
		width: 100%;
		height: 200px;
	}

	.tick {
		font-family: Helvetica, Arial;
		font-size: 0.725em;
		font-weight: 200;
	}

	.tick line {
		stroke: #e2e2e2;
		stroke-dasharray: 2;
	}

	.tick text {
		fill: #ccc;
		text-anchor: start;
	}

	.tick.tick-0 line {
		stroke-dasharray: 0;
	}

	.x-axis .tick text {
		text-anchor: middle;
	}

	.bars rect {
		fill: #a11;
		stroke: none;
		opacity: 0.65;
	}
</style>