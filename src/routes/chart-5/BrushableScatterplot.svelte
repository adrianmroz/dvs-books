<script>
	import { scaleLinear } from 'd3';
	import { createEventDispatcher } from 'svelte';
	import Grid from './Grid.svelte';

	export let data = [];
	export let x;
	export let y;
	export let xs;
	export let ys;
	export let color;

	$: xScale = scaleLinear().domain(xs).range([0, 200]);
	$: yScale = scaleLinear().domain(ys).range([200, 0]);

	// Brush stuff
	// State represents the current state of the brush.
	// If null, that means we're not brushing.
	// Property action can be 'moving' or 'finished'.
	// Also, it has the coordinates of the selected rectangle.
	let state = null;

	const selectionDispatch = createEventDispatcher();

	function dragStart(e) {
		// After drag start, initiate moving state with current coordinates.
		state = {
			action: 'moving',
			x0: e.offsetX,
			x1: e.offsetX,
			y0: e.offsetY,
			y1: e.offsetY
		};
	}

	function dragMove(e) {
		// During mouse movement, first ensure we're in moving state.
		if (state && state.action === 'moving') {
			// Update coordinates of the rectangle.
			state = {
				...state,
				x1: e.offsetX,
				y1: e.offsetY
			};
		}
	}

	function dragStop() {
		// If we're in moving state, change it to finished state.
		if (state && state.action === 'moving') {
			state = { ...state, action: 'finished' };
			return;
		}
		// If we're here and the state is finished, that means user essentially just clicked.
		if (state && state.action === 'finished') {
			state = null;
			return;
		}
	}

	function resetDrag() {
		// If user just clicks, reset the state.
		if (state && Math.abs(state.x0 - state.x1) < 5) {
			state = null;
		}
	}

	// Convert rectangle (pixels) into rectangle of domain values
	function convertToSelection({ x0, x1, y0, y1 }) {
		const minX = xScale.invert(Math.min(x0, x1));
		const maxX = xScale.invert(Math.max(x0, x1));

		// NOTE: Y-scale are always inverted-inverted
		const minY = yScale.invert(Math.max(y0, y1));
		const maxY = yScale.invert(Math.min(y0, y1));

		return data.filter((datum) => {
			const datumX = x(datum);
			const datumY = y(datum);
			return datumX > minX && datumX < maxX && datumY > minY && datumY < maxY;
		});
	}

	$: {
		// Everytime state changes, convert it to selection and dispatch it.
		const selection = state ? convertToSelection(state) : null;
		selectionDispatch('selection', { selection });
	}
</script>

<section class="chart">
	<svg height="200px" width="200px">
		{#each data as datum}
			<circle cx={xScale(x(datum))} cy={yScale(y(datum))} r="2px" fill={color(datum)} />
		{/each}
		{#if state}
			<g>
				<rect
					x={Math.min(state.x0, state.x1)}
					y={Math.min(state.y0, state.y1)}
					width={Math.abs(state.x1 - state.x0)}
					height={Math.abs(state.y1 - state.y0)}
					fill-opacity="0.3"
					fill="hotpink"
				></rect>
			</g>
		{/if}
	</svg>

	<div class="grids">
		<Grid horizontal scale={xScale} count={5} />
		<Grid vertical scale={yScale} count={5} />

		<div
			class="target"
			on:mousedown={dragStart}
			on:mouseup={dragStop}
			on:mousemove={dragMove}
			on:click={resetDrag}
		/>
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

	.target {
		position: absolute;
		width: 200px;
		height: 200px;
	}
</style>
