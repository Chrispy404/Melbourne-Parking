<template>
	<div class="home">
		<h1>The Data</h1>
		{{parkingData}}
	</div>
</template>

<script>
// @ is an alias to /src
// import Home from "@/components/Home.vue";
import axios from "axios";

export default {
	name: "data",
	components: {},
	data() {
		return {
			parkingData: null
		};
	},
	methods: {},
	created() {
		axios
			.get("https://data.melbourne.vic.gov.au/resource/vh2v-4nfs.json")
			.then(response => (this.parkingData = response.data))
			.catch(error => console.log(error));
	}
};
</script>
