<script>
	import Weather from './components/Search.svelte';
	// import RightComp from './components/Info.svelte';
</script>

<svelte:head>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" integrity="sha384-zCbKRCUGaJDkqS1kPbPd7TveP5iyJE0EjAuZQTgFLD2ylzuqKfdKlfG/eSrtxUkn" crossorigin="anonymous">
</svelte:head>

<main>
	<div class="container-fluid" style="padding: 10vh;">
		<div class="row ">
			<Weather/>
		</div>
	</div>
	
</main>

<style>
	/* .row{
		background-color: whitesmoke;
	} */

	main {
		color: white;
		font-family: 'Lato', sans-serif;
	}

</style>