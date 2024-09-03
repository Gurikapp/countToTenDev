<header></header>

<main>
	<div class="stars"></div>
	<div class="twinkling"></div>
	<div class="vertical-line"></div>
	<slot />
</main>

<footer></footer>

<style lang="scss">
	@import '/src/routes/nullstyle.scss';
	main {
		background-color: black;
		height: 200vh;
		.vertical-line {
			width: 3px;
			height: 100vh;
			background-color: #ffffff57;
			position: fixed;
			left: 50%;
			transform: translateX(-50%);
			z-index: 1;
		}
		.stars,
		.twinkling {
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			width: 100%;
			height: 200vh;
			display: block;
			z-index: 0;
		}
		.stars {
			background: #000
				url(https://i.postimg.cc/hhFD7Nkf/apps-47636-13819498107583324-fb06b53a-4974-4acb-85e6-f6f363f85ab6-edec48d0-ec35-4a1f-8bf5-c455987b4a.png)
				repeat top center;
			z-index: 0;
		}
		.twinkling {
			background-image: url(https://i.postimg.cc/CLpNr4PF/twinkling.png);
			animation: 3s twinkle infinite;
			animation-timing-function: linear;
		}
		@keyframes twinkle {
			from {
				backrgound-position: 0;
			}
			to {
				background-position: -100px 0;
			}
		}
	}
</style>
