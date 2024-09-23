<script>
	import { cars } from '$lib/cars.js';
	import { scaleLinear } from 'd3';
	import Grid from './Grid.svelte';

	const data = cars;
	const x = (d) => d.Horsepower;
	const y = (d) => d.Acceleration;
	const color = () => 'hotpink';

	let width = 200;
	let height = 200;

	$: xScale = scaleLinear().domain([0, 250]).range([0, width]);
	$: yScale = scaleLinear().domain([5, 30]).range([height, 0]);
</script>

<div class="chart" bind:clientWidth={width} bind:clientHeight={height}>
	<svg {height} {width} style:position="absolute">
		{#each data as datum}
			<circle cx={xScale(x(datum))} cy={yScale(y(datum))} r="2px" fill={color(datum)} />
		{/each}
	</svg>

	<div style:position="absolute" style:width={`${width}px`} style:height={`${height}px`}>
		<Grid vertical scale={xScale} count={5} />
		<Grid horizontal scale={yScale} count={5} />
	</div>
</div>

<style>
	.chart {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
	}
</style>
