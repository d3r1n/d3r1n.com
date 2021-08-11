<template>
	<div class="tools_container">
		<h1 class="title">
			<span>
				Tools And Languages I Use
			</span>

		</h1>
		<span class="spacer"></span>
		<div class="tools">
			<div class="skill_container">
				<div class="skill" v-for="skill in skills" @click="openSite(skill.link)" :key="skill">
					<p>{{ skill.name }}</p>
				</div>
			</div>

		</div>
	</div>
</template>

<script>

let skills = [
	{
		name: "Rust",	
		link: "https://rust-lang.org/",
	},
	{
		name: "Go",				
		link: "https://golang.com/",
	},
	{
		name: "C++",				
		link: "https://en.wikipedia.org/wiki/C%2B%2B",
	},
	{
		name: "Python",				
		link: "https://python.org/",
	},
	{
		name: "NodeJS",			
		link: "https://nodejs.org/",
	},
	{
		name: "Deno",		
		link: "https://deno.land/",
	},
	{
		name: "HTML5",		
		link: "https://en.wikipedia.org/wiki/HTML"
	},
	{
		name: "SASS",		
		link: "https://sass-lang.com"
	},
	{
		name: "Vue.js",		
		link: "https://vuejs.org/",
	},
	{
		name: "Git",		
		link: "https://git-scm.org"
	},
	{
		name: "React",		
		link: "https://reactjs.org/"
	},
	{
		name: "MongoDB",		
		link: "https://"
	},
	{
		name: "JavaScript",
		class: "long",				
		link: "https://en.wikipedia.org/wiki/JavaScript",
	},
	{
		name: "TypeScript",
		class: "long",		
		link: "https://typescriptlang.org/",
	},
	{
		name: "Docker",		
		link: "https://"
	},
	{
		name: "Linux",		
		link: "https://"
	},
	{
		name: "Vim",		
		link: "https://"
	},
	{
		name: "Vs Code",		
		link: "https://"
	}
]

export default {
	name: "Tools",
	data() {
		return {
			skills: skills
		}
	},
	methods: {
		openSite(link) {
			window.open(link, "_blank")
		}
	}

}
</script>