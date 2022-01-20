<script>
	let city;
	let cloud;
	let temp;
	let windSpeed;
    export const BASE_URL = new URL("https://api.openweathermap.org/data/2.5/weather");
    export const params = {
			units: "metric",
			lang: "id",
			appid: "8a59485fc0098295507e55078609bfee",
		};
    
    

	// const findCity = () => {
    //     let country = ''
        
	// 	if (event.code == "Enter" || event.code == "NumpadEnter") {
	// 		event.preventDefault();
	// 		event.target.value;
	// 		country = event.target.value;
			
	// 		let params = {
	// 			q: `${country}`,
	// 			units: "metric",
	// 			lang: "id",
	// 			appid: "8a59485fc0098295507e55078609bfee",
	// 		};
	// 		BASE_URL.search = new URLSearchParams(params);

	// 		fetch(BASE_URL)
	// 			.then((response) => {
	// 				return response.json();
	// 			})
	// 			.then((data) => {
	// 				if (data.cod == "404") {
	// 					city = "City Not Found";
    //                     cloud = "-";
    //                     temp = "-";
    //                     windSpeed = "-";
	// 				} else {
	// 					city = data.name;
    //                     cloud = data.weather[0].description;
    //                     temp = data.main.temp + " ℃";
    //                     windSpeed = data.wind.speed + " m/s";
	// 				}
	// 			})
	// 			.catch((err) => {
	// 				console.log(err + "error");
	// 			});
	// 		return false;
	// 	}
	// };

	function getLocation() {
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition(showPosition);
		} else {
			console.log("Geolocation is not supported by this browser.");
		}
	}

	function showPosition(position) {
		const lati = position.coords.latitude;
		const long = position.coords.longitude;
        const latLong = {
            lat: `${lati}`,
			lon: `${long}`,
        }
        Object.assign(params, latLong)

		BASE_URL.search = new URLSearchParams(params);

		fetch(BASE_URL)
			.then((response) => {
				return response.json();
			})
			.then((data) => {
                console.log(data)
				if (data.cod == "404") {
					city = "City Not Found";
					cloud = "-";
					temp = "-";
					windSpeed = "-";
				} else {
					city = data.name;
					cloud = data.weather[0].description;
					temp = data.main.temp + " ℃";
					windSpeed = data.wind.speed + " m/s";
				}
			})
			.catch((err) => {
				console.log(err + "error");
			});
	}

	getLocation();

	const find = () => {};
</script>

<!-- <div class="col-md-5 p-0">
	<h1>Right</h1>

	<p>City : {city}</p>
	<p>Condition : {cloud}</p>
	<p>Temperature : {temp}</p>
	<p>Wind speed : {windSpeed}</p>

	<script src="assets/index.js"></script>
</div> -->

<style>
	.col {
		/* background-color: red; */
		color: black;
		background-color: whitesmoke;
		border-radius: 0 50px 50px 0;
	}
</style>
