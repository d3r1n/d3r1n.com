<template>
	<div class="modal" @click="toggle()">
		
		<div class="presence" @click="openSpotify(obj.track_id)" v-if="PresenceType == 1">
			<span id="cover">
				<img :src="obj.LargeImage">
			</span>

			<span class="info">
				<p id="name">{{ obj.name }}</p>
				<p id="text2">By {{ obj.MainText }}</p>
				<p id="text3">In {{ obj.SecondaryText }}</p>
			</span>
		</div>

		<div class="presence" v-if="PresenceType == 2">
			<span id="cover">
				<img :src="obj.LargeImage">
			</span>

			<span class="info">
				<p id="name">{{ obj.name }}</p>
				<p id="text2">{{ obj.MainText }}</p>
				<p id="text3">{{ obj.SecondaryText }}</p>
			</span>
		</div>
	</div>

</template>

<script>
export default {
	name:"PopUp",
	props: ["obj", "PresenceType", "toggle"],
	mounted() {
		const name_ = document.getElementById("name")
		const text2_ = document.getElementById("text2")
		const text3_ = document.getElementById("text3")

		if (name_.innerText.length > 20) {
			name_.innerText = name_.innerText.slice(0, 20) + "-"
		}
		if (text2_.innerText.length > 20) {
			text2_.innerText = text2_.innerText.slice(0, 20) + "-"
		}
		if (text3_.innerText.length > 20) {
			text3_.innerText = text3_.innerText.slice(0, 20) + "-"
		}
	},
	methods: {
		openSpotify(id) {
			window.open("https://open.spotify.com/track/" + id + "/");
		}
	}

}
</script>