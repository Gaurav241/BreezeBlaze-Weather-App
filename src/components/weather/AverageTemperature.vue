<template>
	<area-chart :data="chartInfo" :colors="['#c4daeb']" suffix="°c" min="0" max="50" loading="Loading..."></area-chart>
</template>

<script>
export default {
	name: "AverageTemperature",
	props: {
		weatherDays: Array,
	},
	data() {
		return {
			chartInfo: [],
		};
	},

	created() {
		this.getChartInfo();
	},
	methods: {
		getChartInfo() {
			this.weatherDays.forEach(weather => {
				this.chartInfo.push([this.getDay(weather.dt * 1000), Math.round(weather.main.temp)]);
			});
		},

		getDay(date) {
			return new Date(date).toLocaleString("en-us", {
				weekday: "long",
			});
		},
	},
};
</script>

<style></style>
