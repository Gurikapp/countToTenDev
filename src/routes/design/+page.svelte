<script lang="ts">
	import TimeStemp from '$lib/components/TimeStemp.svelte';
    import items from '$lib/timeStemps.js';
</script>

<section class="timeline">
	<div class="timeline-line" style="z-index: 4;">
		<div class="timeline-timestemps">
			{#each items as item}
				<TimeStemp 
                size="BigDot" 
                dotColor="{item.color}"
                loreText="{item.description}"
                imageUrl="{item.img}"
                year="{item.date}"/>
			{/each}
		</div>
		<div class="vertical-line"></div>
	</div>

	<div class="timeline-line" style="z-index: 3;">
		<div class="timeline-timestemps">
			<TimeStemp />
			<TimeStemp />
			<TimeStemp />
		</div>
		<div class="vertical-line"></div>
	</div>

	<div class="timeline-line" style="z-index: 2;">
		<div class="timeline-timestemps">
			<TimeStemp />
			<TimeStemp />
			<TimeStemp />
		</div>
		<div class="vertical-line"></div>
	</div>
</section>

<style lang="scss">
	.timeline {
		display: flex;
		flex-direction: row;
		justify-content: center;
		gap: 19%;
		padding-top: 400px;
		.timeline-line {
			position: relative;
			&:hover .vertical-line {
				width: 5px;
				background-color: #8e7f55;
			}
			.timeline-timestemps {
				display: flex;
				flex-direction: column;
				gap: 250px;
				align-items: center;
				z-index: 2;
			}
			.vertical-line {
				width: 2px;
				height: 100%;
				background-color: #7f724c;
				position: absolute;
				transform: translateX(-50%);
				left: 50%;
				top: 0%;
				z-index: 1;
				transition: 500ms;
			}
		}
	}
</style>
