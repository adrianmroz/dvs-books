<script>
	import * as d3 from 'd3';
	import { categories } from '$lib';

	export let width = 450;

	const legendWidth = 150;
	$: chartWidth = width - legendWidth;
	const height = 500;

	const xDomain = [0, d3.max(categories, (d) => d.count)];
	const yDomain = categories.map((d) => d.name);

	$: xScale = d3.scaleLinear().domain(xDomain).range([0, chartWidth]);

	const yScale = d3.scaleBand().domain(yDomain).range([height, 0]);

	const plays = categories.find((category) => category.name === 'Play');
	const tooltipY = `${yScale(plays.name) - 60}px`;
	$: tooltipX = `${xScale(plays.count) + legendWidth - 65}px`;
</script>

<div class="container">
	<svg {width} {height} viewBox={`0 0 ${width} ${height}`}>
		<g>
			{#each categories as category}
				<text x="20" y={yScale(category.name) + yScale.bandwidth() / 2} dy="5">{category.name}</text
				>
			{/each}
		</g>
		<g transform={`translate(${legendWidth}, 0)`}>
			{#each categories as category}
				<rect
					x={0}
					y={yScale(category.name) + yScale.bandwidth() / 4}
					width={xScale(category.count)}
					height={yScale.bandwidth() / 2}
					stroke-width="1"
					stroke="black"
					fill="steelblue"
				/>
			{/each}
		</g>
	</svg>
	<div class="tooltip" style:top={tooltipY} style:left={tooltipX}>30 plays!</div>
</div>

<style>
	.container {
		position: relative;
	}

	.tooltip {
		position: absolute;
		font-size: 1.5em;
		color: brown;
		background-color: lightyellow;
		width: 60px;
		padding: 5px 8px;
		border-radius: 5px;
	}
</style>
