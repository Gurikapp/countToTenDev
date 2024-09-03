<script lang="ts">
	import { onMount } from 'svelte';

	let note = '';

	let isPopupVisible = false; // Видимость всплывающего блока


	// Функция для сохранения заметки в localStorage
	const saveNote = () => {
		localStorage.setItem('userNote', note);
	};

	// Загружаем заметку из localStorage при загрузке компонента
	onMount(() => {
		const savedNote = localStorage.getItem('userNote');
		if (savedNote) {
			note = savedNote;
		}
	});

	let popupElement: HTMLDivElement | null = null; // Ссылка на элемент всплывающего блока

	function togglePopup() {
		isPopupVisible = !isPopupVisible;
	}
</script>

<button class="notePopup" on:click={togglePopup}
	><img src="/images/design/2.png" alt="" /></button
>

{#if isPopupVisible}
	<div
		class="note"
		aria-hidden="true"
		bind:this={popupElement}
	>
		<textarea
			class="note-input"
			bind:value={note}
			on:input={saveNote}
			placeholder="Введите заметку..."
			rows="10"
			cols="30"
		></textarea>
	</div>
{/if}

<style lang="scss">
	.notePopup {
		border: none;
		background: none;
		transition: 200ms;
		&:hover {
			background-color: #8e7f554f;
			border-radius: 20%;
		}
		img {
			width: 90px;
		}
	}

	.note {
		position: fixed;
		left: 90px;
		top: 90px;
		z-index: 6;
		background: url('/images/design/textureBack.png');
		width: fit-content;
		height: fit-content;
		padding: 3px;
		.note-input {
			background-color: #060705;
			color: #8e7f55;
			padding: 4px;
			border: 2px solid #8e7f55;
			font-size: 20px;
            z-index: 7;
		}
	}
</style>
