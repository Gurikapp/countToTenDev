<script lang="ts">
	export let dotColor = 'white'; // Цвет точки внутри
	export let size = '100px'; // Размер круга по умолчанию
	export let loreText = 'Ваш текст здесь'; // Текст лора
	export let imageUrl = ''; // URL изображения для показа при наведении
	export let marginRight = '20px'; // Отступ от правого края
	export let marginBottom = '20px'; // Отступ от нижнего края
    export let fontSize = '20px'; 

	let isPopupVisible = false; // Видимость всплывающего блока

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

<button class="container" on:click={togglePopup}>
	<button class="circle" style="--size: {size}; --dot-color: {dotColor};">
		<div aria-hidden="true" class="dot"></div>
	</button>
	<div class="slideLore">
		<div class="sideLore-loreContainer">
			<img class="loreImg" src={imageUrl} alt="" />
			<p class="loreText">{loreText}</p>
		</div>
	</div>
	<div class="text">
		<p class="year" style="--fontSize: {fontSize};">
            <slot></slot>
        </p>
		<!-- Слот для добавления текста -->
	</div>
</button>

{#if isPopupVisible}
	<div
		aria-hidden="true"
		bind:this={popupElement}
		class="lore"
		style="--marginRight: {marginRight}; --marginBottom: {marginBottom};"
		on:mousedown={startDrag}
	>
		<button class="loreClose" on:click={togglePopup}><img alt="" src="/images/Cross.svg" /></button>
		<img src={imageUrl} alt="" class="loreImg" />
		<p class="loreText">{loreText}</p>
	</div>
{/if}

<style lang="scss">
	.container {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
        background: #888888a1;
        border-radius: 9%;
		gap: 20px;
		text-align: center;
		position: relative;
		z-index: 2;
		&:hover .slideLore {
			opacity: 1;
			transform: translateX(10px);
		}
		.circle {
			width: var(--size);
			height: var(--size);
			background: black;
			border: 2px solid rgba(255, 255, 255, 0.781);
			border-radius: 50%;
			position: relative;
			cursor: pointer;
			box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.589);
			transition: 500ms;
			z-index: 3;
			@keyframes circle {
				0% {
					transform: rotate(0deg) translate(calc(var(--size) / 2)) rotate(0deg);
				}
				100% {
					transform: rotate(360deg) translate(calc(var(--size) / 2)) rotate(-360deg);
				}
			}
			&:hover {
				width: calc(var(--size) + 5px);
				height: calc(var(--size) + 5px);
				border: 2.5px solid rgb(255, 255, 255);
				.dot {
					width: calc(var(--size) / 4 + 10px);
					height: calc(var(--size) / 4 + 10px);
				}
			}
		}
		.dot {
			width: calc(var(--size) / 4);
			height: calc(var(--size) / 4);
			border: 3px solid;
			border-color: var(--dot-color);
			border-radius: 50%;
			position: absolute;
			top: 0;
			bottom: 0;
			left: 0;
			right: 0;
			overflow: hidden;
			margin: auto;
			animation: circle 6s linear infinite;
			box-shadow: 0 0 0 3px hsl(from var(--dot-color) h s l / 0.5);
			transition: 500ms;
		}
		.slideLore {
			display: flex;
			flex-direction: column;
			position: absolute;
            width: 500px;
			top: -15%;
			left: 0%;
			margin-left: 28.4px;
			transform: translateX(-5%);
            padding: 10px;
            padding-right: 30px;
            padding-left: 90%;
			background-color: black;
			border-right: 0.5px solid #ffffff57;
			border-top: 0.5px solid #ffffff57;
			border-bottom: 0.5px solid #ffffff57;
			color: white;
			border-top-right-radius: 5px;
			border-bottom-right-radius: 5px;
			opacity: 0;
			transition:
				transform 0.3s ease,
				opacity 0.5s ease;
			pointer-events: none;
			z-index: 2;
			.sideLore-loreContainer {
				display: flex;
				flex-direction: column;
				align-items: center;
				width: 100%;
				.loreImg {
					border-radius: 30px;
					width: 100px;
					height: auto;
					display: block;
				}
				.loreText {
					white-space: pre-line;
					font-size: 13px;
				}
			}
		}
	}
	.text {
		font-size: 1rem;
		color: white;
		position: absolute;
		z-index: 4;
        .year {
		background: linear-gradient(90deg, #ffffff, #c9c9c9, #9e9e9e, #7c7c7c, #464646);
		background-size: 200% auto;
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		animation: textflow 5s linear infinite;
        font-size: var(--fontSize);
	}
	@keyframes textflow {
		from {
			background-position: 0% center;
		}
		to {
			background-position: 200% center;
		}
	}
	}

	.lore {
		display: flex;
		flex-direction: column;
		gap: 20px;
		align-items: center;
		position: fixed;
		width: 15%;
		padding: 15px;
		background-color: #333;
		color: white;
		border-radius: 8px;
		border: 2px solid azure;
		cursor: grab;
		right: var(--marginRight);
		bottom: var(--marginBottom);
		z-index: 4;
		.loreImg {
			max-width: 80%;
			border-radius: 8px;
		}
		.loreClose {
			position: absolute;
			right: 15px;
			top: 15px;
			background: none;
			img {
				width: 15px;
			}
		}
	}
</style>
