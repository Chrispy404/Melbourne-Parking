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
			timer: "",

			// map
			map: null,
			tiles: null,
			tileUrl: "",
			attribution: ""
		};
	},
	methods: {
		getData: function() {
			let config = {
				headers: {
					"X-App-Token": "fAbHz9QDysRavaP5y6iQTkQHo"
				}
			};

			axios
				.get(
					"https://data.melbourne.vic.gov.au/resource/vh2v-4nfs.json",
					config
				)
				.then(response => (this.parkingData = response.data))
				.catch(error => console.log(error))
				.finally(() => this.findSpaces());
		},
		findSpaces: function() {
			this.freeSpaces = [];
			for (let i = 0; i < this.parkingData.length; i++) {
				if (this.parkingData[i].status === "Unoccupied") {
					this.freeSpaces.push(this.parkingData[i]);
				}
			}
		}
	},
	computed: {},
	created() {
		this.getData();
		this.timer = setInterval(this.getData, 60000);
	},
	beforeDestroy() {
		clearInterval(this.timer);
	}
};
</script>

<style>
#melbMap {
	height: 600px;
}
</style>
