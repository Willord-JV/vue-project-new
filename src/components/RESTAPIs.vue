<template>
	<div class="dog-gallery">
		<h2>Hello, {{ userName }}!</h2>

		<div class="image-grid">
			<div
				v-for="(image, index) in images"
				:key="index"
				class="image-container"
			>
				<img :src="image" alt="Dog Image" />
			</div>
		</div>

		<button @click="fetchImages" class="new-images-button">New Images</button>
	</div>
</template>

<script>
	export default {
		props: {
			userName: {
				type: String,
				required: true,
			},
		},
		data() {
			return {
				images: [],
			};
		},
		methods: {
			async fetchImages() {
				try {
					let newImages = new Set();

					while (newImages.size < 6) {
						const response = await fetch(
							"https://dog.ceo/api/breeds/image/random"
						);
						const data = await response.json();

						if (!newImages.has(data.message)) {
							newImages.add(data.message);
						}
					}

					this.images = Array.from(newImages);
				} catch (error) {
					console.error("Error fetching images:", error);
				}
			},
		},
		mounted() {
			this.fetchImages();
		},
	};
</script>
