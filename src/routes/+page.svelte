<script>
    import { space } from "svelte/internal";
	import Calculator from "./Calculator.svelte";

	
	let tabs = ["hello", "Gertings", "ANT215", "CSC427"]
	let current_tab = tabs[0];

</script>

<div class="w-full sm:flex gap-8 h-screen">
	
	<div class="w-full sm:block sm:w-2/5 md:w-54 columns-1 shadow bg-indigo-300 p-2">
		<h1 class="text-white text-center pb-8 pt-5 text-2xl font-sans font-bold underline underline-offset-8">Grade Calculator</h1>
		{#each tabs as tab, i (i)}
			<button class="tab text-left" on:click={() => {current_tab = tab}}>{tab}</button>
		{/each}
		<button class="tab border border-indigo-200">+</button>
	</div>
	
	<div class="w-full mt-8 z-0">
		<Calculator bind:table_name={current_tab}/>
		<p>{current_tab}</p>
	</div>
		

</div>

<style>
.tab {
	@apply bg-indigo-300 text-white font-bold text-lg p-2 px-4 rounded-xl text-center w-full;
}
.tab:hover{
	@apply bg-indigo-400;
}

.tab:active{
	@apply bg-indigo-500;
}
</style>