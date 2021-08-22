<template>
	<div class="modal" @click="toggle()">
		<div class="contanier" @click="openSpotify(obj.track_id)" v-if="PresenceType == 1">
			<div class="presence">
				<span id="cover">
					<img :src="obj.LargeImage">
				</span>
				<span class="info">
					<p id="name">{{ obj.name }}</p>
					<p id="text2">By {{ obj.MainText }}</p>
					<p id="text3">In {{ obj.SecondaryText }}</p>
				</span>
			</div>
		</div>

		<div class="contanier" v-if="PresenceType == 2">
			<div class="presence">
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

		if (name_.innerText.length > 30) {
			name_.innerText = name_.innerText.slice(0, 17) + "-"
		}
		if (text2_.innerText.length > 30) {
			text2_.innerText = text2_.innerText.slice(0, 17) + "-"
		}
		if (text3_.innerText.length > 30) {
			text3_.innerText = text3_.innerText.slice(0, 17) + "-"
		}
	},
	methods: {
		openSpotify(id) {
			window.open("https://open.spotify.com/track/" + id + "/");
		}
	}

}
</script>

<style>

.modal {
	position: absolute;
	display: flex;
	justify-content: center;
	align-items: center;
	background-color: rgba(18, 21, 37, 0.95);
	top:0;
	left:0;
	z-index: 9998;
	width: 100%;
	height: 100%;
}

.modal .container {
	display: flex;
	justify-content: center;
	align-items: center;
	border: 10px solid red;
}

.modal .container .presence {
	display: flex;
	justify-content: space-around;
	align-items: center;
	flex-direction: column;
	animation: smooth-shake 7.5s ease-in-out infinite;
}

#cover img {
	width: 256px;
	height: 256px;
	border-radius: 30%;
}

#name, #text2, #text3 {
	color: #f8f8f2;
	font-size: 20px;
}

#text2 {
	color: #e76f51;
}

#text3 {
	color: #2a9d8f;
}

</style>