<script>
	import Weather from './components/Search.svelte';
	// import RightComp from './components/Info.svelte';
</script>

<svelte:head>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" integrity="sha384-zCbKRCUGaJDkqS1kPbPd7TveP5iyJE0EjAuZQTgFLD2ylzuqKfdKlfG/eSrtxUkn" crossorigin="anonymous">
</svelte:head>

<main>
	<div class="container-fluid main-container">
		<div class="row align-items-stretch weather-row">
			<Weather/>
		</div>
	</div>
	
</main>

<style>
	main {
		color: white;
		font-family: 'Lato', sans-serif;
		min-height: 100vh;
		display: flex;
		align-items: center;
	}

	.main-container {
		padding: 2rem 5vw;
		width: 100%;
		max-width: 1200px;
	}

	.weather-row {
		border-radius: 16px;
		overflow: hidden;
		box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
	}

	@media (max-width: 768px) {
		main {
			display: block;
			min-height: auto;
		}
		.main-container {
			padding: 0;
		}
		.weather-row {
			border-radius: 0;
		}
	}
</style>