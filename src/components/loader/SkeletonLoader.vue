<template>
	<div class="row gy-3">
		<!-- Main section -->
		<div class="col-lg-9 col-12">
			<div class="mb-3">
				<h4 class="mb-3">Current Weather</h4>
				<div class="row gy-3 mb-5">
					<div class="col-lg-4">
						<content-loader width="100%" height="100%" viewBox="0 0 340 84" :speed="2" primaryColor="#1b1a1d"
							secondaryColor="#434049">
							<rect x="0" y="0" rx="30" ry="30" width="100%" height="100%" />
						</content-loader>
					</div>
				</div>
				<h4 class="mb-3">Next Couple of Hours</h4>
				<div class="col-lg-8">
					<div style="height: 100%" class="row row-cols-5">
						<div v-for="index in 5" :key="index" class="col">
							<content-loader width="100%" height="100%" viewBox="0 0 340 84" :speed="2"
								primaryColor="#1b1a1d" secondaryColor="#434049">
								<rect x="0" y="0" rx="30" ry="30" width="100%" height="100%" />
							</content-loader>
						</div>
					</div>
				</div>
			</div>
		</div>
		<!-- Lateral section -->
		<div class="col-lg-3 col-12">
			<div class="mb-3">
				<h4 class="mb-3">Wind</h4>
				<content-loader width="100%" height="100%" viewBox="0 0 340 84" :speed="2" primaryColor="#1b1a1d"
					secondaryColor="#434049">
					<rect x="0" y="0" rx="30" ry="30" width="100%" height="100%" />
				</content-loader>
			</div>
			<div>
				<h4 class="mb-3">Other large cities</h4>
				<div v-for="index in 4" :key="index" class="mb-3">
					<content-loader width="100%" height="100%" viewBox="0 0 340 84" :speed="2" primaryColor="#1b1a1d"
						secondaryColor="#434049">
						<rect x="0" y="0" rx="30" ry="30" width="100%" height="100%" />
					</content-loader>
				</div>
			</div>
		</div>
	</div>
</template>
  
<script>
import { ContentLoader } from "vue-content-loader";

export default {
	components: {
		ContentLoader,
	},
};
</script>
  
<style></style>
  