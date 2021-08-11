<template>
	<div class="modal" @click="toggle()">
		<div class="contanier" @click="openSpotify(obj.track_id)">
			<div class="spotify">
				<span id="art">
					<img :src="obj.album_art_url" :alt="obj.album">
				</span>
				<span class="info">
					<p id="song">{{ obj.song }}</p>
					<p id="artist">By {{ obj.artist }}</p>
					<p id="album">In {{ obj.album }}</p>
				</span>
			</div>
		</div>
	</div>

</template>

<script>

export default {
	name:"PopUp",
	props: ["obj", "toggle"],
	mounted() {
		const song = document.getElementById("song")
		const artist = document.getElementById("artist")
		const album = document.getElementById("album")

		if (song.innerText.length > 30) {
			song.innerText = song.innerText.slice(0, 17).replace(";" || "," || ".", "") + "..."
		}
		if (artist.innerText.length > 30) {
			artist.innerText = artist.innerText.slice(0, 17).replace(";" || "," || ".", "") + "..."
		}
		if (album.innerText.length > 30) {
			album.innerText = album.innerText.slice(0, 17).replace(";" || "," || ".", "") + "..."
		}
	},
	methods: {
		openSpotify(id) {

			window.open("https://open.spotify.com/track/" + this.obj.id + "/");

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

.modal .container .spotify {
	display: flex;
	justify-content: space-around;
	align-items: center;
	flex-direction: column;
	animation: smooth-shake 7.5s ease-in-out infinite;
}

#art img {
	width: 256px;
	height: 256px;
	border-radius: 30%;
}

#song, #artist, #album {
	color: #f8f8f2;
	font-size: 20px;
}

#artist {
	color: #e76f51;
}

#album {
	color: #2a9d8f;
}

</style>