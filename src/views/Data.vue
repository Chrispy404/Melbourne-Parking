<template>
	<div class="home">
		<h1>Find Spaces</h1>
		<button v-on:click="createMap">create Map</button>
		<div id="melbMap"></div>
		<!-- {{freeSpaces}} -->
	</div>
</template>

<script>
// @ is an alias to /src
// import Home from "@/components/Home.vue";
import axios from "axios";
import leaflet from "leaflet";

export default {
	name: "data",
	components: {},
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
		},
		createMap: function() {
			// map name must be the same as Div id name
			this.map = leaflet.map("melbMap").setView([-37.8136, 144.9631], 14);
			// credit where credit's due
			let attribution =
				'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a>';

			// These tiles are al handled by leaflet
			let tileUrl = "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png";
			this.tiles = leaflet.tileLayer(tileUrl, { attribution });
			this.tiles.addTo(this.map);

			this.addMarkers();
		},
		addMarkers: function() {
			for (let i = 0; i < this.freeSpaces.length; i++) {
				let marker = leaflet.marker([
					this.freeSpaces[i].lat,
					this.freeSpaces[i].lon
				]);
				marker.addTo(this.map);
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
