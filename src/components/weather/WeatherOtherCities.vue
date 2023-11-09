<template>
	<div v-if="!loading">
		<div v-for="weather in weatherCities" :key="weather.id" class="card-container city mb-3"
			@click="selectCity(weather)">
			<div class="row">
				<div class="col-lg-8 col-12">
					<span class="alternative-color fs-6">{{ weather.sys.country }}</span><br />
					<h5>{{ weather.name }}</h5>
					<h6>{{ weather.weather[0].description }}</h6>
				</div>
				<div class="col-lg-4 col-12 d-flex flex-column align-items-center">
					<img class="img-fluid"
						:src="'https://openweathermap.org/img/wn/' + weather.weather[0].icon.slice(0, -1) + 'd@2x.png'"
						alt="weather-icon" />
					<span style="font-weight: bold; font-size: 1.6em">{{ Math.round(weather.main.temp) }}<span
							class="gray">°C</span></span>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "WeatherOtherCities",
	data() {
		return {
			//New York, London, Paris, Tokyo
			cities: [
				{ latitude: 40.7127281, longitude: -74.0060152 },
				{ latitude: 51.5073219, longitude: -0.1276474 },
				{ latitude: 48.8445008, longitude: 2.3553084 },
				{ latitude: 35.6828387, longitude: 139.7594549 },
			],
			weatherCities: [],
			loading: true,
		};
	},

	created() {
		this.getWeatherCities();
	},

	methods: {
		async getWeatherCities() {
			this.loading = true;

			this.cities.forEach(city => {
				this.$axios
					.get(
						`${this.$weatherApi}/weather?lat=${city.latitude}&lon=${city.longitude}&units=metric&appid=${this.$apikey}`
					)
					.then(res => {
						this.weatherCities.push(res.data);
					});
			});

			this.loading = false;
		},

		selectCity(weather) {
			let city = { latitude: weather.coord.lat, longitude: weather.coord.lon };

			this.$emit("selectLargeCity", city);
		},
	},
};
</script>

<style scoped>
.card-container.city {
	padding: 20px 30px;
	cursor: pointer;
	transition: 0.3s;
}

.card-container.city:hover {
	translate: 0 -12px;
}
</style>
