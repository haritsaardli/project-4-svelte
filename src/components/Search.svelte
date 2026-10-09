<script>
	let city = "Kota Jakarta"
    let cloud = "light rain"
    let temp = "30°"
    let windSpeed = "15km"
	let country = "";
	let windDir, hum, feelsLike, tempMax, tempMin
	let icon 
	let iconLink
	let aqi = null, aqiLabel = "", aqiColor = "", pm25 = ""
	let bgUrl = "https://picsum.photos/seed/sky/1600/900"

	function weatherToKeyword(conditionId) {
		if (conditionId >= 200 && conditionId < 300) return "thunderstorm";
		if (conditionId >= 300 && conditionId < 400) return "drizzle";
		if (conditionId >= 500 && conditionId < 600) return "rain";
		if (conditionId >= 600 && conditionId < 700) return "snow";
		if (conditionId >= 700 && conditionId < 800) return "fog";
		if (conditionId === 800) return "sunshine";
		if (conditionId > 800) return "clouds";
		return "sky";
	}

    const BASE_URL = new URL("https://api.openweathermap.org/data/2.5/weather");
    const AQI_URL = new URL("https://api.openweathermap.org/data/2.5/air_pollution");
    const API_KEY = "8a59485fc0098295507e55078609bfee";
    const params = {
			units: "metric",
			lang: "en",
			appid: API_KEY,
		};

    const date = new Date()
    const bulan = ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"];
    const weekday = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
    function addZero(i) {
        if (i < 10) {i = "0" + i}
        return i;
    }
    const [month, day, year, hari] = [bulan[date.getMonth()], date.getDate(), date.getFullYear(), weekday[date.getDay()]];
    const [hour, minutes] = [addZero(date.getHours()), addZero(date.getMinutes())];
    const dateNow = `${hour}:${minutes} - ${hari}, ${day} ${month} ${year}`

	function windDirection(degree){
		if (degree>337.5) return 'N';
		if (degree>292.5) return 'NW';
		if(degree>247.5) return 'W';
		if(degree>202.5) return 'SW';
		if(degree>157.5) return 'S';
		if(degree>122.5) return 'SE';
		if(degree>67.5) return 'E';
		if(degree>22.5){return 'NE';}
		return 'N';
	}

	const AQI_LEVELS = [
		{ label: "Good",      color: "#00c853" },
		{ label: "Fair",      color: "#aeea00" },
		{ label: "Moderate",  color: "#ffd600" },
		{ label: "Poor",      color: "#ff6d00" },
		{ label: "Very Poor", color: "#d50000" },
	];

	function fetchAQI(lat, lon) {
		const aqiParams = new URLSearchParams({ lat, lon, appid: API_KEY });
		AQI_URL.search = aqiParams;
		fetch(AQI_URL)
			.then(r => r.json())
			.then(data => {
				const idx = data.list[0].main.aqi; // 1–5
				aqi = idx;
				aqiLabel = AQI_LEVELS[idx - 1].label;
				aqiColor = AQI_LEVELS[idx - 1].color;
				pm25 = data.list[0].components.pm2_5.toFixed(1) + " µg/m³";
			})
			.catch(err => console.log("AQI error:", err));
	}

	const searchCountry = () => {
		if (event.code == "Enter" || event.code == "NumpadEnter") {
			event.preventDefault();
			event.target.value;
			country = event.target.value;

			Object.assign(params, {q: `${country}`});
			BASE_URL.search = new URLSearchParams(params);

			fetch(BASE_URL)
				.then((response) => {
					return response.json();
				})
				.then((data) => {
                    console.log(data)
					if (data.cod == "404" || data.cod == "400") {
						city = "City Not Found";
						cloud = "";
						temp = "";
						windSpeed = "";
						iconLink = ""
						aqi = null; aqiLabel = ""; pm25 = "";
					} else {
						city = data.name;
						cloud = data.weather[0].description;
						temp = Math.round(data.main.temp) + "°";
						windSpeed = data.wind.speed + " m/s";
						windDir = windDirection(data.wind.deg)
						hum = data.main.humidity + ' %'
						icon = data.weather[0].icon
						iconLink = `https://openweathermap.org/img/wn/${icon}@2x.png`

						feelsLike = Math.round(data.main.feels_like) + '°C'
						tempMax = Math.round(data.main.temp_max) + '°C'
						tempMin = Math.round(data.main.temp_min) + '°C'

						const keyword = weatherToKeyword(data.weather[0].id);
						bgUrl = `https://picsum.photos/seed/${keyword}${Date.now()}/1600/900`;

						fetchAQI(data.coord.lat, data.coord.lon);
					}
				})
				.catch((err) => {
					console.log(err + "error");
				});
			event.target.value = ""
			return false;
		}
	};

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
					city = "GPS not found";
					cloud = "-";
					temp = "-";
					windSpeed = "-";
				} else {
					city = data.name;
					cloud = data.weather[0].description;
					temp = Math.round(data.main.temp) + "°";
					windSpeed = data.wind.speed + " m/s";
					windDir = windDirection(data.wind.deg)
					hum = data.main.humidity + ' %'
					icon = data.weather[0].icon
					iconLink = `https://openweathermap.org/img/wn/${icon}@2x.png`
					
					feelsLike = Math.round(data.main.feels_like) + '°C'
					tempMax = Math.round(data.main.temp_max) + '°C'
					tempMin = Math.round(data.main.temp_min) + '°C'

					const keyword = weatherToKeyword(data.weather[0].id);
					bgUrl = `https://picsum.photos/seed/${keyword}${Date.now()}/1600/900`;

					fetchAQI(lati, long);
				}
			})
			.catch((err) => {
				console.log(err + "error");
			});
	}

	getLocation();
</script>

<svelte:head>
	<title>{city}</title>
	<link rel="icon" type="image/png" href="{iconLink}">
</svelte:head>

<div class="col-md-7 p-0 left panel-left">
	<div class="maintxt">
		<div class="gradientEffect">
			<img class="bkg" src={bgUrl} alt="weather background" />
		</div>
		<div class="overlay-text">
            <div class="row m-0">
                <div class="col p-0">
                    <h3>forecast</h3>
                </div>
            </div>
		
            <div class="weather-main-row">
                <div class="d-flex align-items-center flex-wrap">
                    <h1 class="m-0 temp mr-3">{temp}</h1>
                    <div class="weather-city-date mr-3">
                        <h1 class="m-0 city">{city}</h1>
                        <h4 class="m-0 date">{dateNow}</h4>
                    </div>
                    <div class="weather-condition-badge d-flex align-items-center ml-auto">
						{#if iconLink}
							<img src={iconLink} alt="weather icon" class="weather-icon mr-1">
						{/if}
                        <p class="mb-0 cloud">{cloud}</p>
                    </div>
                </div>
            </div>
		</div>
	</div>
</div>

<!-- RIGHT -->
<div class="col-md-5 p-4 right panel-right">

    <input
		class=""
		on:keyup|preventDefault={searchCountry}
		type="text"
		placeholder="Find City"
	/>

	<div class="row mt-3">
		<div class="col">
			<p class="section-title">Weather Detail</p>
		</div>
	</div>

	<div class="row mt-1">
		<div class="col condition">
			<p>City</p>
			<p>Condition</p>
			<p>Temperature</p>
			<p>Wind speed</p>
			<p>Humidity</p>
		</div>
		<div class="col text-right">
			<p>{city}</p>
			<p>{cloud}</p>
			<p>{temp}</p>
			<p>{windSpeed}{windDir ? ' ' + windDir : ''}</p>
			<p>{hum || '-'}</p>
		</div>
	</div>

	<hr class="my-3">

	<div class="row">
		<div class="col">
			<p class="section-title">Air Quality</p>
		</div>
	</div>

	<div class="row mt-1">
		<div class="col condition">
			<p>AQI</p>
			<p>Status</p>
			<p>PM2.5</p>
		</div>
		<div class="col text-right">
			<p>{aqi !== null ? aqi : '-'}</p>
			<p>
				{#if aqi !== null}
					<span class="aqi-badge" style="background-color: {aqiColor};">{aqiLabel}</span>
				{:else}
					-
				{/if}
			</p>
			<p>{pm25 || '-'}</p>
		</div>
	</div>

	<hr class="my-3">

	<div class="row">
		<div class="col">
			<p class="section-title">Temperature Detail</p>
		</div>
	</div>

	<div class="row mt-1">
		<div class="col condition">
			<p>Feels Like</p>
			<p>Max</p>
			<p>Min</p>
			
		</div>
		<div class="col text-right">
			<p>{feelsLike || '-'}</p>
			<p>{tempMax || '-'}</p>
			<p>{tempMin || '-'}</p>
		</div>
	</div>

	<!-- <script src="assets/index.js"></script> -->
</div>

<style>
	hr {
		border-top: 1px solid #417d91;
		margin-top: 1rem;
		margin-bottom: 1rem;
	}
	.left {
		text-shadow: 0 0 10px #00000087;
	}

	.right {
		background-color: #1a3e4a;
	}

	.panel-right {
		padding: clamp(1.5rem, 3vw, 2.5rem) !important;
	}

	.panel-right p {
		margin-bottom: 0.25rem;
		font-size: 15px;
	}

	.section-title {
		font-weight: 500;
		letter-spacing: 2px;
		margin-bottom: 0.4rem;
	}

	.weather-icon {
		width: 45px;
		height: 45px;
	}

	.condition {
		color: #cecece;
	}

	input{
		background-color: #ff000000;
		border: none;
		border-bottom: 2px solid #417d91;
		color: #5bb4d3;
		font-size: 18px;
		width: 100%;
	}
	::placeholder {
		color: #417d91;
	}

	.city{
		font-size: 40px;
		letter-spacing: 4px;
	}
	.temp{
		font-size: 80px;
	}
	.date{
		font-size: 15px;
		letter-spacing: 2px;
	}
	.cloud{
		font-size: 15px;
		letter-spacing: 2px;
	}

	.panel-left {
		display: flex;
		flex-direction: column;
		position: relative;
		min-height: 420px;
	}

	.bkg {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
	}

	.gradientEffect {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.gradientEffect::after {
		content: "";
		left: 0;
		top: 0;
		position: absolute;
		width: 100%;
		height: 100%;
		display: block;
		background: #163b48b8;
	}

	/* weather info stays at the bottom of the left panel */
	.weather-main-row {
		margin-top: auto;
	}

	h3 {
		font-weight: 400;
		letter-spacing: 5px;
		color: whitesmoke;
		margin: 0;
	}
	.maintxt {
		position: relative;
		flex: 1 1 auto;
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
	.overlay-text {
		position: relative;
		z-index: 1;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		width: 100%;
		height: 100%;
		flex: 1 1 auto;
		padding: 2.25rem;
		box-sizing: border-box;
	}

	.aqi-badge {
		display: inline-block;
		padding: 2px 10px;
		border-radius: 12px;
		font-size: 13px;
		font-weight: 600;
		color: #111;
		letter-spacing: 1px;
	}

	/* Tablet: stack the panels before the two-column layout gets cramped. */
	@media (min-width: 577px) and (max-width: 991px) {
		.panel-left,
		.panel-right {
			flex: 0 0 100%;
			max-width: 100%;
		}
		.panel-left {
			min-height: clamp(300px, 42vw, 400px);
		}
		.panel-right {
			padding: clamp(1.5rem, 4vw, 2.25rem) !important;
		}
		.overlay-text {
			padding: clamp(1.5rem, 4vw, 2.25rem);
		}
	}

	/* Wide tablet / compact laptop: keep the two columns balanced. */
	@media (min-width: 992px) and (max-width: 1199px) {
		.panel-left {
			min-height: 400px;
		}
		.panel-right {
			padding: 1.5rem !important;
		}
		.overlay-text {
			padding: 1.75rem;
		}
		.temp {
			font-size: 68px;
		}
		.city {
			font-size: 32px;
			letter-spacing: 2px;
		}
	}

	/* Mobile (≤ 576px) */
	@media (max-width: 576px) {
		.panel-left {
			min-height: 260px;
		}
		.overlay-text {
			padding: 1.5rem 1rem;
		}
		.temp {
			font-size: 48px;
		}
		.city {
			font-size: 22px;
			letter-spacing: 2px;
		}
		.date {
			font-size: 11px;
			letter-spacing: 1px;
		}
		.cloud {
			font-size: 12px;
			margin-top: -10px;
		}
		h3 {
			font-size: 14px;
			letter-spacing: 3px;
		}
		.panel-right {
			padding: 1.5rem !important;
		}
	}
</style>
