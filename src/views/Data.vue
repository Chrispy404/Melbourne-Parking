<template>
	<div class="home">
		<h1>The Data</h1>
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
			// const mapAccessToken =
			// 	"pk.eyJ1IjoiY3BhbmV0dGEiLCJhIjoiY2syOXdhb3VhMGZ3ZjNrbzFob3BvdGI1NyJ9.BvHYgMwnLli4l2uBvZk0OQ";

			this.map = leaflet.map("melbMap").setView([-37.8136, 144.9631], 14);
			let attribution =
				'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a>';

			let tileUrl = "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png";
			this.tiles = leaflet.tileLayer(tileUrl, { attribution });
			this.tiles.addTo(this.map);
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
