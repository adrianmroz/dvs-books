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
</script>

<svg {width} {height} viewBox={`0 0 ${width} ${height}`}>
	<g>
		{#each categories as category}
			<text x="20" y={yScale(category.name) + yScale.bandwidth() / 2} dy="5">{category.name}</text>
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
