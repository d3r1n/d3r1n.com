<template>
	<div class="footer">
		<div id="footer_content">
			<p>This Website is Open-Source @ <a class="hover-underline" id="footer_github" href="https://github.com/d3r1n/d3r1n.com" target="_blank">Github</a></p>
			<p id="x11">Made with <span id="footer_love">Love</span></p>
		</div>
	</div>
</template>

<script>
export default {
	name: "Footer"
}
</script>

<style>
.footer {
	display: flex;
	align-items: center;
	justify-content: center;
	margin: 10px;
	height: 65px;
	width: 90%;
	border-radius: 20px;
	background: rgba(21,22,25, 0.7);
}

#footer_content {
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: row;
	margin-top: 5px;
	margin-bottom: 5px;
}

#footer_love {
	color: #fb3640;
	font-size: 18px;
	font-weight: bold;
}

.footer p {
	color: #f8f8f2;
}

#x11 {
	margin-left: 10px;
}


#footer_github {
	color: #bd93f9;
}

</style>