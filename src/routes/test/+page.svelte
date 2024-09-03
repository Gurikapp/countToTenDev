<section>
	<button class="timestemp-container BigDot">
		<div class="timestemp">
			<div class="timestemp-border">
				<div class="timestemp-square"></div>
			</div>
		</div>
	</button>
</section>

<style lang="scss">
    @import '/src/routes/nullstyle.scss';
	section {
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: black;
		height: 100vh;
		.timestemp-container {
			display: flex;
			align-items: center;
			justify-content: center;
			background: none;
			border: none;
			position: relative;
			.timestemp {
				display: flex;
				align-items: center;
				justify-content: center;
				background: url('/images/design/textureBack.png');
				transform: rotate(45deg);
				padding: 10%;
				z-index: 3;
				width: 30px; //ШИРИНА
				height: 30px; //ВЫСОТА
				&:hover {
					animation: boxShadowAnimation 5s ease infinite;
					@keyframes boxShadowAnimation {
						0% {
							box-shadow:
								0 0 20px #ff00cc,
								0 0 40px #ff00cc,
								0 0 60px #ff00cc,
								0 0 80px #ff00cc;
						}
						50% {
							box-shadow:
								0 0 20px #00ccff,
								0 0 40px #00ccff,
								0 0 60px #00ccff,
								0 0 80px #00ccff;
						}
						100% {
							box-shadow:
								0 0 20px #ff00cc,
								0 0 40px #ff00cc,
								0 0 60px #ff00cc,
								0 0 80px #ff00cc;
						}
					}
				}
				.timestemp-border {
					display: flex;
					align-items: center;
					justify-content: center;
					width: 100%;
					height: 100%;
					background-color: #b2bcbd; //ЦВЕТ ВНУТРИ ТОЧКИ
					padding: 20%;
					.timestemp-square {
						display: block;
						width: 100%;
						height: 100%;
						background: url('/images/design/textureBack.png');
						border: none;
					}
				}
			}
		}
	}
</style>
