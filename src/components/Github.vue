<template>
	<div class="github_container">
		<h1 class="github_title">Github Repositories</h1>
		<div class="repo" v-for="repo in repos" :key="repo.name" >
			<div class="repo_top">
				<span id="repo_name" @click="openRepo(repo.link)" class="hover-underline">{{ repo.name }}</span>
				<span id="stars">
					{{ repo.stars }}
				</span>
				<svg id="star_svg" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="star" class="star" viewBox="0 0 576 512">
					<path d="M259.3 17.8L194 150.2 47.9 171.5c-26.2 3.8-36.7 36.1-17.7 54.6l105.7 103-25 145.5c-4.5 26.3 23.2 46 46.4 33.7L288 439.6l130.7 68.7c23.2 12.2 50.9-7.4 46.4-33.7l-25-145.5 105.7-103c19-18.5 8.5-50.8-17.7-54.6L382 150.2 316.7 17.8c-11.7-23.6-45.6-23.9-57.4 0z"/>
				</svg>
			</div>
			<span id="repo_description">{{ repo.description }}</span>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Github',
	data() {
		return {
			repos: []
		}
	},
	async mounted() {

		let user = "d3r1n" // change it with your own Github username
		const result = await fetch(`https://api.github.com/users/${user}/repos`);
		let repos = filterRepos(await result.json());

		for (let repo of repos) { // loop through the repositories
			let obj = new Object
			obj.name = repo.name
			obj.stars = repo.stargazers_count
			obj.link = repo.html_url

			try {
				if (repo.description.length > 15) {
					obj.description = repo.description.slice(0,30) + ".."
				}
				if (repo.name.length > 10) obj.name = repo.name.slice(0,10) + ".."
			}
			catch (e) {
				
				obj.description = repo.description
				obj.name = repo.name
			
			}

			this.repos.push(obj)
		}

		function filterRepos(repos) { // filter repositories according to date (newest first)
			const found = new Array;

			for (let repo of repos) {
				repo.created_at = new Date(repo.created_at);
				found.push(repo);
			}

			const sortRepo = found.sort((a, b) => b.created_at - a.created_at);

			return sortRepo.slice(0, 12);
		}
	},
	methods: {
		openRepo(url) {
			window.open(url, "_blank")
		}
	}
}
</script>