<script lang="ts">
    export let size = "SmallDot";
	export let dotColor = 'black'; 
	export let loreText = 'Лор лорный'; 
	export let imageUrl = ''; 
	export let year = 'год';


	let isPopupVisible = false; // Видимость всплывающего блока
	export let marginRight = '20px'; 
	export let marginBottom = '20px'; 

	// Состояние перетаскивания
	let isDragging = false;
	let dragOffset = { x: 0, y: 0 }; // Смещение курсора относительно блока при начале перетаскивания
	let popupElement: HTMLDivElement | null = null; // Ссылка на элемент всплывающего блока

	function togglePopup() {
		isPopupVisible = !isPopupVisible;
	}

	function startDrag(event: MouseEvent) {
		if (event.button === 0 && popupElement) {
			// Проверка, что это левый клик и элемент существует
			event.preventDefault();
			isDragging = true;

			// Вычисляем смещение курсора относительно блока при начале перетаскивания
			const rect = popupElement.getBoundingClientRect();
			dragOffset = {
				x: event.clientX - rect.left,
				y: event.clientY - rect.top
			};

			window.addEventListener('mousemove', drag);
			window.addEventListener('mouseup', stopDrag);
		}
	}

	function drag(event: MouseEvent) {
		if (isDragging && popupElement) {
			const newX = event.clientX - dragOffset.x;
			const newY = event.clientY - dragOffset.y;

			// Обновляем положение блока
			popupElement.style.right = `auto`;
			popupElement.style.bottom = `auto`;
			popupElement.style.left = `${newX}px`;
			popupElement.style.top = `${newY}px`;
		}
	}

	function stopDrag(event: MouseEvent) {
		if (event.button === 0) {
			isDragging = false;
			window.removeEventListener('mousemove', drag);
			window.removeEventListener('mouseup', stopDrag);
		}
	}
</script>

<div class="container {size}">
	<button class="timestemp-container" style="--dot-color: {dotColor};" on:click={togglePopup}>
		<div class="timestemp">
			<div class="timestemp-border">
				<div class="timestemp-square"></div>
			</div>
		</div>

		<div class="lorePopup">
			<div class="lorePopup-lineborder">
				<div class="lorePopup-lineborder-secondLayer">
					<div class="lorePopup-lineborder-thirdLayer">
						<div class="lorePopup-header">
							<p class="lorePopup-header-year">{year}</p>
						</div>
						<div class="lorePopup-lore">
							<p class="lorePopup-lore-p">
								{loreText}
							</p>
						</div>
						<div class="lorePopup-Img">
							<img src="/images/{imageUrl}" alt="" />
						</div>
					</div>
				</div>
			</div>
		</div>
	</button>

	<div class="timestemp-year">
		<p>{year}</p>
	</div>
</div>

{#if isPopupVisible}
	<div
		aria-hidden="true"
		bind:this={popupElement}
		class="lorePopupDrag"
		style="--marginRight: {marginRight}; --marginBottom: {marginBottom};"
		on:mousedown={startDrag}
	>
		<div class="lorePopupDrag-lineborder">
			<div class="lorePopupDrag-lineborder-secondLayer">
				<div class="lorePopupDrag-lineborder-thirdLayer">
					<button class="lorePopupDrag-closeBtn" on:click={togglePopup}
						><svg
							width="15"
							height="15"
							viewBox="0 0 41 41"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<path
								fill-rule="evenodd"
								clip-rule="evenodd"
								d="M40.1361 3.86233C40.9538 2.9792 40.9538 1.54548 40.1361 0.662347C39.0458 -0.220782 37.6829 -0.220782 36.8652 0.662347L20.2384 17.1611L3.88413 0.662347C2.79385 -0.220782 1.431 -0.220782 0.613284 0.662347C-0.204428 1.54548 -0.204428 2.9792 0.613284 3.86233L17.2401 20.361L0.613284 36.8625C-0.204428 37.7456 -0.204428 39.1766 0.613284 40.0597C1.431 40.9456 2.79385 40.9456 3.88413 40.0597L20.2384 23.561L36.8652 40.0597C37.6829 40.9456 39.0458 40.9456 40.1361 40.0597C40.9538 39.1766 40.9538 37.7456 40.1361 36.8625L23.5092 20.361L40.1361 3.86233Z"
								fill="#8E7F55"
							/>
						</svg></button
					>
					<div class="lorePopupDrag-header">
						<p class="lorePopupDrag-header-year">{year}</p>
					</div>
					<div class="lorePopupDrag-lore">
						<p class="lorePopupDrag-lore-p">
							{loreText}
						</p>
					</div>
					<div class="lorePopupDrag-Img">
						<img src="/images/{imageUrl}" alt="" />
					</div>
				</div>
			</div>
		</div>
	</div>
{/if}

<style lang="scss">
	.BigDot{
		.timestemp{
			width: 30px;
			height: 30px;
			.timestemp-square {
				display: block;
			}
		}
	}

	.SmallDot{
		.timestemp{
			width: 20px;
			height: 20px;
			.timestemp-square {
				display: none;
			}
		}
	}

	.container {
		position: relative;
		.timestemp-container {
			display: flex;
			align-items: center;
			justify-content: center;
			background: none;
			border: none;
			position: relative;
			&:hover .lorePopup {
				opacity: 1;
				transform: translateX(1%);
			}
			.timestemp {
				display: flex;
				align-items: center;
				justify-content: center;
				background: url('/images/design/textureBack.png');
				transform: rotate(45deg);
				padding: 10%;
				z-index: 3;
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
						25% {
							box-shadow:
								0 0 20px #3333ff,
								0 0 40px #3333ff,
								0 0 60px #3333ff,
								0 0 80px #3333ff;
						}
						50% {
							box-shadow:
								0 0 20px #00ccff,
								0 0 40px #00ccff,
								0 0 60px #00ccff,
								0 0 80px #00ccff;
						}
						75% {
							box-shadow:
								0 0 20px #ff6600,
								0 0 40px #ff6600,
								0 0 60px #ff6600,
								0 0 80px #ff6600;
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
					background-color: var(--dot-color);
					padding: 20%;
					.timestemp-square {
						width: 100%;
						height: 100%;
						background: url('/images/design/textureBack.png');
						border: none;
					}
				}
			}
		}

		.lorePopup {
			display: flex;
			align-items: center;
			justify-content: center;
			position: absolute;
			left: 5%;
			z-index: 2;
			transform: translateX(-5%);
			opacity: 0;
			transition:
				transform 0.3s ease,
				opacity 0.5s ease;
			pointer-events: none;
			width: 350px;
			background: url('/images/design/textureBack.png');
			clip-path: polygon(
				0 0,
				2% 3%,
				2% 50%,
				2% 97%,
				0 100%,
				49% 100%,
				100% 100%,
				98% 97%,
				98% 50%,
				98% 3%,
				100% 0,
				48% 0
			);
			.lorePopup-lineborder {
				display: flex;
				align-items: center;
				justify-content: center;
				width: 100%;
				height: 100%;
				margin: 3px 10px;
				background-color: #060705;
				.lorePopup-lineborder-secondLayer {
					display: flex;
					align-items: center;
					justify-content: center;
					background: url('/images/design/textureBack.png');
					width: 100%;
					height: 100%;
					margin: 3px 3px;
					.lorePopup-lineborder-thirdLayer {
						display: flex;
						flex-direction: column;
						align-items: center;
						background-color: #060705;
						margin: 3px 3px;
						padding: 3px;
						width: 100%;
						height: 100%;
						.lorePopup-header {
							display: flex;
							flex-direction: column;
							color: #8e7f55;
							.lorePopup-header-year {
								font-size: 30px;
							}
						}
						.lorePopup-lore {
							color: #8e7f55;
							white-space: pre-line;
							.lorePopup-lore-p {
								font-size: 20px;
							}
						}
						.lorePopup-Img {
							background: url('/images/design/textureBack.png');
							padding: 3px;
							margin-top: 10px;
							img {
								width: 100%;
								max-height: 200px;
							}
						}
					}
				}
			}
		}

		.timestemp-year {
			position: absolute;
			color: #8e7f55;
			left: 190%;
			top: 0%;
		}
	}

	.lorePopupDrag {
		display: flex;
		align-items: center;
		justify-content: center;
		position: fixed;
		cursor: grab;
		right: var(--marginRight);
		bottom: var(--marginBottom);
		z-index: 5;
		width: 350px;
		background: url('/images/design/textureBack.png');
		clip-path: polygon(
			0 0,
			2% 3%,
			2% 50%,
			2% 97%,
			0 100%,
			49% 100%,
			100% 100%,
			98% 97%,
			98% 50%,
			98% 3%,
			100% 0,
			48% 0
		);
		.lorePopupDrag-lineborder {
			display: flex;
			align-items: center;
			justify-content: center;
			width: 100%;
			height: 100%;
			margin: 3px 10px;
			background-color: #060705;
			.lorePopupDrag-lineborder-secondLayer {
				display: flex;
				align-items: center;
				justify-content: center;
				background: url('/images/design/textureBack.png');
				width: 100%;
				height: 100%;
				margin: 3px 3px;
				.lorePopupDrag-lineborder-thirdLayer {
					display: flex;
					flex-direction: column;
					position: relative;
					align-items: center;
					background-color: #060705;
					margin: 3px 3px;
					padding: 3px;
					width: 100%;
					height: 100%;
					.lorePopupDrag-closeBtn {
						position: absolute;
						right: 1%;
						top: 1%;
						background: none;
					}
					.lorePopupDrag-header {
						display: flex;
						flex-direction: column;
						color: #8e7f55;
						text-align: center;
						.lorePopupDrag-header-year {
							font-size: 30px;
						}
					}
					.lorePopupDrag-lore {
						color: #8e7f55;
						white-space: pre-line;
						text-align: center;
						.lorePopup-lore-p {
							font-size: 20px;
						}
					}
					.lorePopupDrag-Img {
						background: url('/images/design/textureBack.png');
						padding: 3px;
						margin-top: 10px;
						img {
							width: 100%;
							max-height: 200px;
						}
					}
				}
			}
		}
	}
</style>
