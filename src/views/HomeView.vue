<template>
	<div class="row align-items-center">
		<div class="col-3">
			<content-loader v-if="loading" viewBox="0 0 340 84" :speed="2" primaryColor="#1b1a1d" secondaryColor="#434049">
				<rect x="40" y="8" rx="3" ry="3" width="140" height="11" />
				<circle cx="14" cy="14" r="14" />
			</content-loader>
			<h5 v-else style="display: inline">
				<i class="bi bi-geo-alt" style="margin-right: 8px"></i>{{ weather.sys.country }}, {{ weather.name }}
			</h5>
		</div>
		<div class="col-6">
			<search-cords-component @citySearch="getWeather"></search-cords-component>
		</div>
		<div class="col-3">
			<a href="https://github.com/Gaurav241" target="_blank">
				<img class="float-end gaurav-icon me-2" width="55" src="@/assets/images/gaurav.jpg" alt="gaurav" />
			</a>
		</div>
	</div>
	<div class="mb-4"></div>

	<skeleton-loader v-if="loading"></skeleton-loader>
	<div v-else class="row gy-3">
		<!-- Main section -->
		<div class="col-lg-9 col-12">
			<div class="mb-3">
				<div class="form-check form-switch">
					<input class="form-check-input" type="checkbox" id="sliderCheckbox" v-model="sliderValue"
						@change="toggleView" />
					<label class="form-check-label" for="sliderCheckbox">
						{{ isNextWeek ? "This Week Weather" : "Today's Weather" }}
					</label>
				</div>
				<div>
					<div v-if="isNextWeek">
						<h3 class="mb-3" data-aos="fade-right">Week Weather</h3>
						<div class="row gy-3 mb-5" data-aos="fade-down">
							<div class="col-lg-11">
								<weather-info-today :weather="weather"></weather-info-today>
							</div>
						</div>
						<h3 class="mb-3" data-aos="fade-right">Next 5 Days</h3>
						<div class="col-lg-11">
							<weather-next-days-component :weatherDays="weatherDays"></weather-next-days-component>
						</div>
						<div>
							<h3 class="mb-3" data-aos="fade-right">Average Week Temperature</h3>
							<average-temperature :weatherDays="weatherDays" data-aos="zoom-in"></average-temperature>
						</div>
					</div>
					<div v-else>
						<h3 class="mb-3" data-aos="fade-right">Current Weather</h3>
						<div class="row gy-3 mb-5" data-aos="fade-down">
							<div class="col-lg-11">
								<weather-info :weather="weather"></weather-info>
							</div>
						</div>
						<h3 class="mb-3" data-aos="fade-right">Next Couple of Hours</h3>
						<div class="col-lg-11">
							<weather-next-hours-component :weatherHours="weatherHours"></weather-next-hours-component>
						</div>
					</div>
				</div>
			</div>
		</div>
		<!-- Lateral section -->
		<div class="col-lg-3 col-12">
			<div class="mb-3">
				<h4 class="mb-3" data-aos="fade-left">Wind</h4>
				<weather-chance-rain :windSpeed="windSpeed" :currentSpeed="weather.wind.speed"
					:WindDirection="weather.wind.deg"></weather-chance-rain>
			</div>
			<div>
				<h4 class="mb-3" data-aos="fade-left">Other large cities</h4>
				<weather-other-cities @selectLargeCity="getWeather"></weather-other-cities>
			</div>
		</div>
	</div>
</template>

<script>
import WeatherInfo from "@/components/weather/WeatherInfo.vue";
import WeatherInfoToday from "@/components/weather/WeatherInfoToday.vue";
import SearchCordsComponent from "@/components/SearchCords.vue";
import WeatherNextDaysComponent from "@/components/weather/WeatherNextDays.vue";
import WeatherNextHoursComponent from "@/components/weather/WeatherNextHours.vue";
import WeatherOtherCities from "@/components/weather/WeatherOtherCities.vue";
import WeatherChanceRain from "@/components/weather/WeatherWindSpeed.vue";
import AverageTemperature from "@/components/weather/AverageTemperature.vue";

import SkeletonLoader from "@/components/loader/SkeletonLoader.vue";
import { ContentLoader } from "vue-content-loader";

export default {
	name: "HomeView",
	components: {
		WeatherInfo,
		WeatherInfoToday,
		SearchCordsComponent,
		WeatherNextDaysComponent,
		WeatherNextHoursComponent,
		WeatherOtherCities,
		WeatherChanceRain,
		AverageTemperature,
		ContentLoader,
		SkeletonLoader,
	},
	data() {
		return {
			weather: {},
			weatherDays: [],
			windSpeed: [],
			loading: true,
			isNextWeek: false,
			weatherHours: [],
			sliderValue: 0,
			//Cords Delhi
			city: { latitude: 28.656120, longitude: 77.240727 },
		};
	},

	created() {
		this.getWeather(this.city);
	},

	methods: {
		toggleView() {
			this.isNextWeek = this.sliderValue == 1;
		},

		async getWeather(city) {
			this.loading = true;
			this.city = city;

			await this.$axios
				.get(
					`${this.$weatherApi}/weather?lat=${city.latitude}&lon=${city.longitude}&units=metric&appid=${this.$apikey}`
				)
				.then(res => {
					this.weather = res.data;
				});

			this.getWeatherNextDays(city);
			this.getWeatherNextHours(city);
			this.getWindSpeed(city);
		},

		async getWeatherNextHours(city) {
			// Clear the array
			this.weatherHours = [];

			await this.$axios
				.get(
					`${this.$weatherApi}forecast?lat=${city.latitude}&lon=${city.longitude}&units=metric&appid=${this.$apikey}`
				)
				.then((res) => {
					// Filter and store hourly weather data for the next couple of hours
					const currentTime = new Date().getTime() / 1000;
					res.data.list.forEach((weather) => {
						if (weather.dt > currentTime && this.weatherHours.length < 5) {
							this.weatherHours.push(weather);
						}
					});
				});
		},

		async getWeatherNextDays(city) {
			let auxWeather = [];
			//Empty the array
			this.weatherDays = [];

			await this.$axios
				.get(
					`${this.$weatherApi}forecast?lat=${city.latitude}&lon=${city.longitude}&units=metric&appid=${this.$apikey}`
				)
				.then(res => {
					auxWeather = res.data.list;
				});

			let auxDate = "";
			const currenDate = new Date().toLocaleString("en-us", {
				day: "numeric",
				month: "long",
				year: "2-digit",
			});
			auxWeather.forEach(weather => {
				let weatherDate = new Date(weather.dt * 1000).toLocaleString("en-us", {
					day: "numeric",
					month: "long",
					year: "2-digit",
				});

				if (auxDate != weatherDate && weatherDate != currenDate) {
					this.weatherDays.push(weather);
				}

				auxDate = weatherDate;
			});
		},

		async getWindSpeed(city) {
			let allWeather = [];

			//Clear speedWind
			this.windSpeed = [];

			await this.$axios
				.get(
					`${this.$weatherApi}forecast?lat=${city.latitude}&lon=${city.longitude}&units=metric&appid=${this.$apikey}`
				)
				.then(res => {
					allWeather = res.data.list;
				});

			allWeather.forEach(weather => {
				if (this.windSpeed.length < 5)
					this.windSpeed.push([
						new Date(weather.dt_txt).toLocaleString("en-us", {
							weekday: "short",
							hour: "2-digit",
							minute: "2-digit",
						}),
						weather.wind.speed,
					]);
			});

			this.loading = false;
		},
	},
};
</script>
<style scoped>
a {
	text-decoration: none;
	color: white;
}

a:hover {
	text-decoration: underline;
}

.gaurav-icon {
	border-radius: 50%;
	overflow: hidden;
}

.form-check-input[type="checkbox"] {
	width: 90px;
	/* Set the width of the checkbox */
	height: 40px;
	/* Set the height of the checkbox */
}

.form-check-label {
	font-size: 30px;
	/* Set the font size of the label text */
	margin-left: 10px;
	/* Add some space between the checkbox and the label */
}
</style>
