<template>
	<div class="about">
		<div class="description">
			<p class="text">
				<span id="welcome">Welcome to my website, I'm <span id="name">d3r1n</span>!</span>
				<span>
					I'm a 14 y.o self taught programmer...
				</span>
				<span>
					And I'm always open to learn new things!!!
				</span>
			</p>
		</div>

		<div class="profile" @click="toggle">
			<div id="status" :style="{ background: lanyard.status }">
				<img src="https://www.howtodoanything.org/wp-content/uploads/2018/03/clownfish.jpg" v-if="lanyard.avatar == undefined">
				<img :src="`https://cdn.discordapp.com/avatars/${lanyard.id}/${lanyard.avatar}.png?size=512`" v-else>
			</div>
			
			<div id="info" v-if="lanyard.listening_spotify">
				<span><span id="name">{{ lanyard.name }}</span><span id="tag">#{{ lanyard.tag }}</span></span>
				<span id="spt">Listening To Spotify...</span>
			</div>

			<div id="info" v-else>
				<span><span id="name">{{ lanyard.name }}</span><span id="tag">#{{ lanyard.tag }}</span></span>
			</div>
		</div>
	</div>
</template>

<script>

export default {
	name: 'About',
	props: ["lanyard", "toggle"],
	mounted() {
		const profile = document.querySelector(".profile")

		if (this.lanyard.listening_spotify) {
			profile.classList.add("scale")
		}
	}
}
</script>
