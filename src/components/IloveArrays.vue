<template>
	<div class="ilove-arrays">
		<!-- Display the submitted name -->
		<h2>Hello, {{ userName }}</h2>

		<!-- Input section to accept comma-delimited numbers -->
		<div class="input-section">
			<input
				v-model="numberInput"
				placeholder="Enter numbers (0-9) separated by commas"
				type="text"
			/>
			<button @click="submitNumbers">Submit</button>
		</div>

		<!-- Display the results if any exist -->
		<div class="results-section" v-if="sortedResults.length > 0">
			<h3>Results:</h3>
			<ul>
				<li>(Number - Count)</li>
				<li v-for="(item, index) in sortedResults" :key="index">
					{{ item.number }} - {{ item.count }}
				</li>
			</ul>
		</div>
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
				numberInput: "", // To capture user input
				results: {}, // Store counts of each unique number
			};
		},
		computed: {
			// Sort results by count (descending), then by number (descending)
			sortedResults() {
				return Object.entries(this.results)
					.map(([number, count]) => ({ number: parseInt(number), count })) // Convert to objects
					.sort((a, b) => b.count - a.count || b.number - a.number); // Sort by count and then number
			},
		},
		methods: {
			// Handles submission of the numbers and populates results
			submitNumbers() {
				if (!this.numberInput) return; // Ensure the input is not empty

				// Parse the input into an array of numbers
				const numbers = this.numberInput
					.split(",") // Split by commas
					.map((num) => num.trim()) // Remove extra whitespace
					.filter((num) => num.match(/^[0-9]$/)); // Ensure valid numbers (0-9)

				// Reset results
				this.results = {};

				// Count occurrences of each number
				numbers.forEach((num) => {
					if (this.results[num]) {
						this.results[num]++;
					} else {
						this.results[num] = 1;
					}
				});

				// Clear the input field after submission
				this.numberInput = "";
			},
		},
	};
</script>
