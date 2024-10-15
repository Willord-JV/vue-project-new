<template>
	<div id="app">
		<div class="header">
			<h1>My Vue App</h1>
		</div>
		<nav class="left-bar">
			<ul>
				<li><a href="#" @click.prevent="showComponent('Home')">Home</a></li>
				<li>
					<a
						href="#"
						@click.prevent="navigateTo('IloveArrays')"
						:class="{ disabled: isNameEmpty }"
						>I love Arrays</a
					>
				</li>
				<li>
					<a
						href="#"
						@click.prevent="navigateTo('RestAPIs')"
						:class="{ disabled: isNameEmpty }"
						>REST API</a
					>
				</li>
			</ul>
		</nav>
		<div class="main-content">
			<component
				:is="currentComponent"
				@update:name="updateName"
				:userName="name"
			></component>
		</div>
	</div>
</template>

<script>
	import { ref, computed } from "vue";
	import Home from "./components/Home.vue";
	import IloveArrays from "./components/IloveArrays.vue";
	import RestAPIs from "./components/RESTAPIs.vue";

	export default {
		components: {
			Home,
			IloveArrays,
			RestAPIs,
		},
		setup() {
			const currentComponent = ref("Home");
			const name = ref("");

			const showComponent = (component) => {
				currentComponent.value = component;
			};

			const updateName = (newName) => {
				name.value = newName; // Update the name based on Home component input
			};

			const isNameEmpty = computed(() => {
				return name.value.trim() === ""; // Check if the name input is empty
			});

			const navigateTo = (component) => {
				if (!isNameEmpty.value) {
					showComponent(component); // Only navigate if name is not empty
				}
			};

			return {
				currentComponent,
				showComponent,
				updateName,
				isNameEmpty,
				navigateTo,
				name,
			};
		},
	};

	document.addEventListener("DOMContentLoaded", () => {
		const themeToggle = document.getElementById("theme-toggle");
		const currentTheme = localStorage.getItem("theme") || "light";
		document.documentElement.setAttribute("data-theme", currentTheme);

		themeToggle.addEventListener("click", () => {
			const theme =
				document.documentElement.getAttribute("data-theme") === "dark"
					? "light"
					: "dark";
			document.documentElement.setAttribute("data-theme", theme);
			localStorage.setItem("theme", theme);
			themeToggle.textContent =
				theme === "dark" ? "Switch to Light Mode" : "Switch to Dark Mode";
		});

		themeToggle.textContent =
			currentTheme === "dark" ? "Switch to Light Mode" : "Switch to Dark Mode";
	});
</script>
