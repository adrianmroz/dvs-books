<script>
	import Chart from './Chart.svelte';
	let width;
</script>

<main>
	<article>
		<h1>Wole Soyinka</h1>
		<p>
			Akinwande Oluwole Babatunde "Wole" Soyinka CFR (/ˈwoʊleɪ sɔɪˈ(j)ɪŋkə, - ʃɔɪˈ-/ WOH-lay
			s(h)oy-(Y)ING-kə; Yoruba: Akínwándé Olúwọlé Babátúndé "Wọlé" Ṣóyíinká, pronounced [wɔlé
			ʃójĩnká]; born 13 July 1934) is a Nigerian playwright, novelist, poet, and essayist in the
			English language. He was awarded the 1986 Nobel Prize in Literature for his "wide cultural
			perspective and... poetic overtones fashioning the drama of existence",[2] the first
			sub-Saharan African to win the Prize in literature.[3][a]
		</p>
		<div class="container" bind:clientWidth={width}>
			<Chart {width} />
		</div>
	</article>
</main>

<style>
	main {
		display: grid;
		place-content: center;
	}
	article {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		max-width: 80ch;
	}
	.container {
		width: 100%;
		position: relative;
	}
</style>
