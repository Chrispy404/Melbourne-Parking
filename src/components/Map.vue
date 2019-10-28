<template>
	<div>
		<div id="melbMap"></div>
		{{displaySpaces}}
	</div>
</template>

<script>
import leaflet from "leaflet";

export default {
	name: "map",
	props: {
		freeSpaces: Array
	},
	data() {
		return {
			displaySpaces: "",
			map: null,
			tiles: null
		};
	},
	methods: {
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
		},
		updateMarkers: function() {
			for (let i = 0; i < this.freeSpaces.length; i++) {
				let marker = leaflet.marker([
					this.freeSpaces[i].lat,
					this.freeSpaces[i].lon
				]);
				marker.addTo(this.map);
			}
		},
		getData: function() {
			this.displaySpaces = this.freeSpaces;
		}
	},
	computed: {},
	mounted() {
		this.createMap();
		console.log("Free spaces: " + this.freeSpaces);
		this.displaySpaces = this.freeSpaces;
	},
	watch: {
		freeSpaces() {
			this.updateMarkers();
		}
	}
};
</script>

<style>
#melbMap {
	height: 600px;
}
</style>
