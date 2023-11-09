<template>
	<div class="d-flex justify-content-between mt-3">
		<div data-aos="fade-right">
			<h5 style="display: inline; margin-right: 5px">
				Speed: {{ currentSpeed }}
			</h5>
			<span class="gray fs-6">m/s</span>
		</div>
		<div class="d-flex align-items-center" data-aos="fade-left">
			<h5 style="display: inline; margin-right: 5px">
				Direction: {{ WindDirection }}°
			</h5>
		</div>
	</div>
	<bar-chart :data="windSpeed" :colors="['#c4daeb']" :min="0" :max="30" suffix="m/s" data-aos="zoom-in"></bar-chart>
</template>

<script>
export default {
	name: "ChanceRain",
	props: {
		windSpeed: Array,
		currentSpeed: Number,
		WindDirection: Number,
	},

	computed: {
		currentHour() {
			return new Date().toLocaleString("en-us", { hour: "2-digit", minute: "2-digit" });
		},
	},
};
</script>

<style></style>
