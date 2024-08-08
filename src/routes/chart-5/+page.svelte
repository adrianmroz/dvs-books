<script>
	import { cars } from '$lib/cars.js';
	import Scatterplot from './Scatterplot.svelte';
	import BrushableScatterplot from './BrushableScatterplot.svelte';
	import { scaleOrdinal, schemePaired } from 'd3';

	const data = cars;
	const origins = new Set(data.map((d) => d.Origin));

	const colorScale = scaleOrdinal()
		.domain(...origins)
		.range(schemePaired);

	const color = (d) => colorScale(d.Origin);

	// Color scale that takes into account the selection.
	$: effectiveColor = (d) => {
		if (selection && !selection.includes(d)) {
			return 'grey';
		}
		return color(d);
	};

	// Show selected cars or all cars if no brush is applied
	$: effectiveDataset = selection ?? data;

	let selection = null;

	function onSelection(e) {
		selection = e.detail.selection;
	}
</script>

<main>
	<BrushableScatterplot
		on:selection={onSelection}
		{data}
		{color}
		xs={[0, 250]}
		ys={[5, 30]}
		x={(d) => d.Horsepower}
		y={(d) => d.Acceleration}
	/>

	<Scatterplot
		{data}
		color={effectiveColor}
		xs={[0, 250]}
		ys={[5, 30]}
		x={(d) => d.Displacement}
		y={(d) => d.Acceleration}
	/>

	<Scatterplot
		data={effectiveDataset}
		{color}
		xs={[0, 50]}
		ys={[5, 30]}
		x={(d) => d.Miles_per_Gallon}
		y={(d) => d.Acceleration}
	/>
</main>

<style>
	main {
		display: flex;
		flex-wrap: wrap;
	}
</style>
