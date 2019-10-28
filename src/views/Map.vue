<template>
	<div class="home">
		<h1>Find Spaces</h1>
		<!-- <button v-on:click="createMap">create Map</button> -->
		<Map v-bind:freeSpaces="freeSpaces"></Map>
		<!-- {{freeSpaces}} -->
	</div>
</template>

<script>
// @ is an alias to /src
import Map from "@/components/Map.vue";
import axios from "axios";

export default {
	name: "data",
	components: {
		Map
	},
	data() {
		return {
			parkingData: null,
			freeSpaces: [],

			// map
			map: null,
			tiles: null,
			tileUrl: "",
			attribution: ""
		};
	},
	methods: {
		findSpaces: function() {
			for (let i = 0; i < this.parkingData.length; i++) {
				if (this.parkingData[i].status === "Unoccupied") {
					this.freeSpaces.push(this.parkingData[i]);
				}
			}
		}
	},
	computed: {},
	created() {
		axios
			.get("https://data.melbourne.vic.gov.au/resource/vh2v-4nfs.json")
			.then(response => (this.parkingData = response.data))
			.catch(error => console.log(error))
			.finally(() => this.findSpaces());
	}
};
</script>

<style>
#melbMap {
	height: 600px;
}
</style>
