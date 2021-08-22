<template>
	<div class="center">
		<About :lanyard="lanyard" :toggle="toggle"/>
		<Tools />
		<PopUp :obj="SpotifyObject" :toggle="toggle" v-if="isOpen"/>
		<Github />
		<Footer />
	</div>
</template>

<script>

const CONFIG = {
	DISCORD_ID: "704758931343278162"
}

import About from './components/About.vue'
import Tools from './components/Tools.vue'
import PopUp from './components/PopUp.vue'
import Github from './components/Github.vue'
import Footer from './components/Footer.vue'

export default {
	name: 'App',
	components: {
		About,
		Tools,
		PopUp,
		Github,
		Footer
	},
	data() {
		return { 
			SpotifyObject: {},
			isOpen: false,
			lanyard: {
				id: String,
				name: "d3r1n",
				avatar: undefined,
				tag: "3199",
				status: "#99aab5",
				listening_spotify: true,
			},
		}
	},
	async mounted() {
		const OPCODES = {
			INFO: 0,
			HELLO: 1,
			INIT: 2,
			HEARTBEAT: 3,
		};

		const lanyard = new WebSocket("wss://api.lanyard.rest/socket");

		// On Message
		lanyard.onmessage = ({ data }) => {
			const reqDataJSON = JSON.parse(data);

			if (reqDataJSON.op == OPCODES.HELLO) {
				// Identify
				lanyard.send(
				JSON.stringify({
					op: OPCODES.INIT,
					d: {
					subscribe_to_id: CONFIG.DISCORD_ID,
					},
				})
				);

				console.log(reqDataJSON.d.heartbeat_interval)

				// Interval
				setInterval(function () {
				lanyard.send(
					JSON.stringify({
					op: OPCODES.HEARTBEAT,
					})
				);
				}, reqDataJSON.d.heartbeat_interval);
			} 
			else if (reqDataJSON.op == OPCODES.INFO) {

				const Colors = {
					online: "#43b581",
					offline: "#99aab5",
					idle: "#faa61a",
					dnd: "#f04747",
				};

				if (reqDataJSON.t == "INIT_STATE") {
					const user = reqDataJSON.d;

					this.lanyard = {
						id: user.discord_user.id,
						name: user.discord_user.username,
						avatar: user.discord_user.avatar,
						tag: user.discord_user.discriminator,
					}

					if (user.listening_to_spotify == true) {
						this.SpotifyObject = user.spotify
						this.lanyard.listening_spotify= user.listening_to_spotify
						document.querySelector(".profile").style.cursor = "pointer"
					}


					this.lanyard.status = Colors[user.discord_status]

				}

				else if (reqDataJSON.t == "PRESENCE_UPDATE") {
					const user = reqDataJSON.d;
					this.lanyard.status = Colors[user.discord_status]
					this.SpotifyObject = user.spotify

					console.log("%c LANYARD_PRESENCE UPDATE", "color: pink; background: black; font-weight: bold;")
				}
			}
		}
	},

	methods: {
		toggle() {
			if(this.lanyard.listening_spotify == true) {
				this.isOpen = !this.isOpen;
			}
			console.log(this.isOpen)
		}
	}
}
</script>

<style>

</style>
