<template>
	<form @submit.prevent="submitForm">
		<div class="form-group">
			<label for="name">Name</label>
			<input id="name" v-model="name" name="name" />
		</div>
		<button type="submit">Submit</button>
	</form>
</template>

<script>
	export default {
		data() {
			return {
				name: "",
			};
		},
		methods: {
			submitForm() {
				console.log(`Submitted Name: ${this.name}`);
				this.$emit("update:name", this.name);
			},
		},
	};
</script>
