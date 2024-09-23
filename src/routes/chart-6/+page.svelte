<script>
	import Scatterplot from './Scatterplot.svelte';
</script>

<main>
	<div>
		<h1>Chart 6</h1>
		<h3>Some details about chart, and description and lorem ipsum</h3>
	</div>
	<div class="chart">
		<Scatterplot />
	</div>
	<div class="details">
		<h3>Details</h3>
		<p>
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed ut purus eget nunc. Nullam nec
			nunc nec nunc. Nullam nec nunc nec nunc. Nullam nec nunc nec nunc. Nullam nec nunc nec nunc.
			Nullam nec nunc nec nunc. Nullam nec nunc nec nunc. Nullam nec nunc nec nunc. Nullam nec nunc
			nec nunc. Nullam nec nunc nec nunc. Nullam nec nunc nec nunc. Nullam nec nunc nec nunc. Nullam
			nec nunc nec nunc.
		</p>
	</div>
</main>

<style>
	main {
		display: grid;
		grid-template-columns: 1fr min(65ch, 100%) 1fr;
		grid-row-gap: 32px;
	}

	main > * {
		grid-column: 2;
	}

	@media (min-width: 1200px) {
		main {
			grid-template-columns: 1fr 65ch 65ch 1fr;
			grid-column-gap: 32px;
		}

		main > * {
			grid-column: 2 / 4;
		}

		.chart {
			grid-column: 2;
		}

		.details {
			grid-column: 3;
		}
	}

	h1,
	h3 {
		font-family: Inter, sans-serif;
		font-weight: 800;
	}

	h1 {
		text-align: center;
		font-size: 2.43rem;
	}

	h3 {
		font-size: 1.56rem;
	}

	p {
		font-family: Georgia, serif;
		font-size: 1.125rem;
		line-height: 1.7;
	}

	.chart {
		position: relative;
		height: 350px;
	}

	.details {
		padding: 15px;
		border-radius: 12px;
		background-color: #f4f4f4;
	}
</style>
