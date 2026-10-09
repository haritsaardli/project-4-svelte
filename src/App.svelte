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
		min-height: 100svh;
		display: flex;
		align-items: center;
	}

	.main-container {
		padding: clamp(1.25rem, 4vw, 3.5rem);
		width: 100%;
		max-width: 1500px;
		margin-inline: auto;
	}

	.weather-row {
		border-radius: 20px;
		overflow: hidden;
		box-shadow: 0 16px 48px rgba(0, 0, 0, 0.38);
	}

	@media (max-width: 991px) {
		main {
			align-items: flex-start;
		}
		.main-container {
			max-width: 860px;
			padding: clamp(1rem, 3vw, 1.75rem);
		}
		.weather-row {
			border-radius: 18px;
		}
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