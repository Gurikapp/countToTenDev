<script lang="ts">
	import Timestemp from '$lib/components/TimestempOld.svelte';
	import Btn from '$lib/components/TimeStemp.svelte';
  import items from '$lib/timeStemps.js';
  import {Bezdna, Zizn, Aringot, Aconit, Umfana, Kamelia, Geroi, Magi, Astei, Lili, Riel, Smert, Les, Statisfet, Pelagi, Panteon, Vedmi, Ludi, Muzi, Alisa} from '$lib/godColors.js';

  let filteredItems = items;
  function handleColorChange(color: string | null) {
    if (color) filteredItems = items.filter((i)=>i.color==color);
    else filteredItems = items;
  }
</script>

<section class="timeline">
	<!-- <Timestemp
		dotColor="yellow"
		size="70px"
		loreText="рассказ о событии"
		imageUrl="/images/0ec092ce0927d02dfba0d6d8bbf3c177.jpg"
		fontSize="20px"
	>
		1012
	</Timestemp>

  <button class="btn" on:click={()=>handleColorChange(Bezdna)}>CHANGE</button>
  <button class="btn" on:click={()=>handleColorChange(null)}>Reset</button> -->

  {#each filteredItems as item}
	<Timestemp
		dotColor="{item.color}"
		size="70px"
		loreText="{item.description}"
		imageUrl="/images/{item.img}"
		fontSize="10px"
	>
  {item.date}
	</Timestemp>
  {/each}
</section>

<style lang="scss">
	.timeline {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 50px;
		padding-top: 30px;
    .btn{
      z-index: 5;
      background-color: aliceblue;
    }
	}
</style>
