<template>
	<div class="content-page">
		<router-view />
	</div>
	<footer>
		<!-- Copyright -->
		<div class="text-center p-3" style="background-color: rgba(0, 0, 0, 0.2)">
			© {{ currentYear }} Copyright:
			<a href="https://github.com/Gaurav241">Gaurav</a>
		</div>
		<!-- Copyright -->
	</footer>
</template>

<script>
export default {
	name: "App",
	data() {
		return {
			currentYear: new Date().getFullYear()
		};
	},
	methods: {},

	computed: {},

	mounted() {
		// Refresh the page every 5 minutes (300,000 milliseconds)
		setInterval(() => {
			window.location.reload();
		}, 300000);
	}
};
</script>

<style>
#app {
	background-color: #101014;
	min-height: 100vh;
	color: white;
	overflow-x: hidden;
	font-size: 0.7em;
	font-family: Sans Infanty;
}

.content-page {
	padding: 25px;
}

footer {
	background-color: #1b1a1d;
}

footer a {
	color: #bbd8ec;
}

@font-face {
	font-family: Sans Infanty;
	src: url("./assets/fonts/ProductSansInfanity.ttf");
}

.card-container {
	border-radius: 30px;
	background-color: #1b1a1d;
	height: 100%;
	overflow: hidden;
}

/* Cards */
.card-container .card-header,
.card-container .card-content {
	padding: 20px 30px;
}

.card-container .card-header {
	border-radius: 30px 30px 0 0;
}

/* Card info */
.card-container.info {
	color: #101014;
	background-color: #bbd8ec;
}

.card-container .card-header.info {
	background-color: #adcade;
	font-weight: bold;
	font-size: 1.2em;
}

/*Card normal */

/* Lines */
.line-gray {
	width: 100%;
	border-bottom: 1px solid #929b9f;
}

span.gray {
	font-size: 0.9em;
	color: #929b9f;
}

.alternative-color {
	color: #929b9f;
}

.sub-yellow {
	color: #ffc62d;
}
</style>
