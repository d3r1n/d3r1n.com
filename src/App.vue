<template>
	<div class="center">
		<About :lanyard="lanyard" :PresenceType="PresenceType" :toggle="toggle"/>
		<Tools />
		<PopUp :obj="PresenceObject" :PresenceType="PresenceType" :toggle="toggle" v-if="isOpen"/>
		<Github />
		<Footer />
	</div>
</template>

<script>

// CONFIG: Change the DISCORD_ID with your own DISCORD ID
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
		// All the Global Objects - Change the name tag with your own tag and discord name (Fallback name and tag)
		return { 
			PresenceObject: {},
			PresenceType: 0,
			isOpen: false,
			lanyard: {
				id: String,
				name: "d3r1n",
				avatar: undefined,
				tag: "1413",
				status: "#99aab5",
			},
		}
	},
	async mounted() {

		console.log("%c DEBUG CONSOLE ", "color: green; background: black; font-weight: bold; font-size: 15px;")

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

					this.lanyard = { // initialize the basic lanyard object 
						id: user.discord_user.id,
						name: user.discord_user.username,
						avatar: isGif(user.discord_user.id, user.discord_user.avatar),
						tag: user.discord_user.discriminator,
					}

					// Checks if the avatar is gif; if it is returns the .gif extension; if it is not, returns .png extension
					function isGif(id,avatar) {
						
						if (avatar.startsWith("a_")) return `https://cdn.discordapp.com/avatars/${id}/${avatar}.gif?size=256`
						else return `https://cdn.discordapp.com/avatars/${id}/${avatar}.png?size=512`
					}
					
					// if activity array is not empty loop over activities and return PresenceObject
					if (user.activities.length > 0) { 
						for (let activity of user.activities) {

							if (activity.type == 0) { // Visual Studio Code Presence
								let ActivityObject = {
									name: activity.name,
									LargeImage: `https://cdn.discordapp.com/app-assets/${activity.application_id}/${activity.assets.large_image}.png?size=512`,
									MainText: activity.details,
									SecondaryText: activity.state,
								}

								this.PresenceType = 2
								this.PresenceObject = ActivityObject

								document.querySelector(".profile").style.cursor = "pointer"
								document.querySelector(".profile").classList.add("scale")

								break
							}

							else if (activity.type == 2) { // Spotify Presence
								let SpotifyObject = {
									name: activity.details,
									LargeImage: `https://i.scdn.co/image/${activity.assets.large_image.replace("spotify:", "")}`,
									MainText: activity.state,
									SecondaryText: activity.assets.large_text,
									track_id: activity.sync_id
								}

								this.PresenceType = 1
								this.PresenceObject = SpotifyObject

								document.querySelector(".profile").style.cursor = "pointer"
								document.querySelector(".profile").classList.add("scale")

								break
							}

							else if (activity.type == 4 && user.activities.length == 1) { // if there is not spotify or vscode activity returns the custom status with custom Emoji support 
								this.PresenceType = 3
								document.querySelector(".profile").style.cursor = "auto"
								document.querySelector(".profile").classList.remove("scale")
								if (activity.emoji == undefined) {
									this.lanyard.custom_status = activity.state
								}
								else if (activity.emoji != undefined && activity.emoji.id == undefined && activity.emoji.name != undefined) {
									this.lanyard.custom_status = `${activity.emoji.name} ${activity.state}`
								}
								else if (activity.emoji != undefined && activity.emoji.name != undefined) {
									this.PresenceType = 4
									this.lanyard.custom_status = activity.state

									if (activity.emoji.id != undefined && activity.emoji.animated) {
										this.lanyard.emoji = `https://cdn.discordapp.com/emojis/${activity.emoji.name}.gif`
									}

									else this.lanyard.emoji = `https://cdn.discordapp.com/emojis/${activity.emoji.name}.png`
								}

								break
							}
						}
					}

					else { // otherwise returns nothing and Sets the Type 0
						this.PresenceType = 0
						document.querySelector(".profile").style.cursor = "auto"
						document.querySelector(".profile").classList.remove("scale")
					}

					this.lanyard.status = Colors[user.discord_status]	

				}

				else if (reqDataJSON.t == "PRESENCE_UPDATE") { // update presence ( Every Heartbeat )
					const user = reqDataJSON.d;
					this.lanyard.status = Colors[user.discord_status]

					if (user.activities.length > 0) { 
						for (let activity of user.activities) {
							if (activity.type == 0) {
								let ActivityObject = {
									name: activity.name,
									LargeImage: `https://cdn.discordapp.com/app-assets/${activity.application_id}/${activity.assets.large_image}.png?size=512`,
									MainText: activity.details,
									SecondaryText: activity.state,
								}
								this.PresenceType = 2
								this.PresenceObject = ActivityObject

								document.querySelector(".profile").style.cursor = "pointer"
								break
							}
							else if (activity.type == 2) {
								let SpotifyObject = {
									name: activity.details,
									LargeImage: `https://i.scdn.co/image/${activity.assets.large_image.replace("spotify:", "")}`,
									MainText: activity.state,
									SecondaryText: activity.assets.large_text,
									track_id: activity.sync_id
								}
								this.PresenceType = 1
								this.PresenceObject = SpotifyObject

								document.querySelector(".profile").style.cursor = "pointer"
								break
							}
							else if (activity.type == 4 && user.activities.length == 1) { // if there is not spotify or vscode activity returns the custom status with custom Emoji support 
								this.PresenceType = 3
								document.querySelector(".profile").style.cursor = "auto"
								document.querySelector(".profile").classList.remove("scale")
								if (activity.emoji == undefined) {
									this.lanyard.custom_status = activity.state
								}
								else if (activity.emoji != undefined && activity.emoji.id == undefined && activity.emoji.name != undefined) {
									this.lanyard.custom_status = `${activity.emoji.name} ${activity.state}`
								}
								else if (activity.emoji != undefined && activity.emoji.name != undefined) {
									this.PresenceType = 4
									this.lanyard.custom_status = activity.state

									if (activity.emoji.id != undefined && activity.emoji.animated) {
										this.lanyard.emoji = `https://cdn.discordapp.com/emojis/${activity.emoji.name}.gif`
									}

									else this.lanyard.emoji = `https://cdn.discordapp.com/emojis/${activity.emoji.name}.png`
								}

								break
							}
						}
					}

					else {
						this.PresenceType = 0
						document.querySelector(".profile").style.cursor = "auto"
					}

					console.log("%c LANYARD_PRESENCE UPDATE", "color: pink; background: black; font-weight: bold;")
				}
			}
		}
	},

	methods: {
		toggle() { // if presence type is vscode or spotify toggle is enabled
			if(this.PresenceType == 1 || this.PresenceType == 2) {
				this.isOpen = !this.isOpen;
			}
			console.log(this.isOpen)
		}
	}
}
</script>

<style>

</style>
