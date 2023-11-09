<template>
	<form class="d-flex align-items-center" role="search" @submit.prevent="getCords">
		<div class="search-city-input">
			<button type="submit" class="btn-search"><i class="bi bi-search"></i></button>
			<input type="text" placeholder="Search city..." aria-label="Search" v-model="location" minlength="3"
				maxlength="30" />
		</div>
	</form>
</template>

<script>
export default {
	name: "SerchCords",
	data() {
		return {
			location: "",
			cords: { latitude: "", longitude: "" },
		};
	},

	methods: {
		async getCords() {
			let error = false;

			await this.$axios.get(`${this.$locationApi}${this.location}`).then(res => {
				if (res.data.results.length != 0) {
					this.cords.latitude = res.data.results[0].geometry.lat;
					this.cords.longitude = res.data.results[0].geometry.lng;
				} else {
					error = true;
				}
			});

			if (error) {
				this.$toast.error("No cities with that name have been found!");
				return;
			}

			this.$emit("citySearch", this.cords);
			this.location = "";
		},
	},
};
</script>

<style>
.search-city-input {
	width: 100%;
	background-color: #1e1e1e;
	border-radius: 25px;
	color: white;
	height: 42px;
	outline: none;
	transition: 0.3s;
	font-size: 1.4em;
	display: flex;
	padding-left: 8px;
}

.search-city-input input {
	padding-left: 8px;
	background: none;
	outline: none;
	border: none;
	width: 100%;
	color: #5b5b5b;
}

.search-city-input input:focus {
	color: white;
}

.search-city-input .btn-search {
	outline: none;
	color: #d2d2d2;
	background: none;
	border: none;
	transition: 0.3s ease-in-out;
}

.search-city-input .btn-search:hover {
	color: #7c7c7c6b;
}
</style>
