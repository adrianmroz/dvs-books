<script>
	export let count = undefined;
	export let horizontal = false;
	export let vertical = false;
	export let scale;

	const VERTICAL = {};
	const HORIZONTAL = {};

	$: orientation = vertical ? VERTICAL : HORIZONTAL;

	$: _ticks = scale.ticks(count);

	$: style =
		orientation === HORIZONTAL
			? (n, i) => `width: 100%; height: 0; top: ${scale(n, i)}px`
			: (n, i) => `width: 0; height: 100%; left: ${scale(n, i)}px`;
</script>

<div class="my-grid">
	{#each _ticks as tick, i}
		<div class="my-grid-item" style={style(tick, i)}>
			<div class={`grid-line ${vertical ? 'vertical' : 'horizontal'}`}></div>
		</div>
	{/each}
</div>

<style>
	.my-grid-item {
		position: absolute;
		left: 0;
		top: 0;
	}

	.vertical {
		height: 100%;
		border-left: 1px dashed #ccc;
	}

	.horizontal {
		width: 100%;
		border-bottom: 1px dashed #ccc;
	}

	.grid-line {
		position: absolute;
		left: 0;
		bottom: 2px;
		font-family: sans-serif;
		font-size: 14px;
		color: #999;
	}
</style>
