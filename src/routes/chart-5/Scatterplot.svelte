<script>
	import { scaleLinear } from 'd3';
	import Grid from './Grid.svelte';

	export let data = [];
	export let x;
	export let y;
	export let xs;
	export let ys;
	export let color = () => 'hotpink';

	$: xScale = scaleLinear().domain(xs).range([0, 200]);
	$: yScale = scaleLinear().domain(ys).range([200, 0]);
</script>

<section class="chart">
	<svg height="200px" width="200px">
		{#each data as datum}
			<circle cx={xScale(x(datum))} cy={yScale(y(datum))} r="2px" fill={color(datum)} />
		{/each}
	</svg>

	<div class="grids">
		<Grid horizontal scale={xScale} count={5} />
		<Grid vertical scale={yScale} count={5} />
	</div>
</section>

<style>
	.chart {
		position: relative;
		width: 200px;
		height: 200px;
		margin: 50px;
	}

	svg {
		position: absolute;
	}

	div {
		position: absolute;
		width: 200px;
		height: 200px;
	}
</style>
