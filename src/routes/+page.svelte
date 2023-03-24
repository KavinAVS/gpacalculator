<script>
	import { Icon, ChevronDown, ChevronUp, PencilSquare, Trash } from "svelte-hero-icons";
	import Calculator from "./Calculator.svelte";

	
	let tabs = Object.keys(localStorage);
	let current_tab = tabs[0];
	let dropdown = false;

</script>

<div class="w-full sm:flex gap-8 h-screen">
	
	<div class="w-full sm:block sm:w-2/5 md:w-54 columns-1 shadow bg-indigo-300 p-2">
		<h1 class="hidden sm:block text-white text-center pb-8 pt-5 text-2xl font-sans font-bold underline underline-offset-8">Grade Calculator</h1>
		<div><button class="sm:hidden tab flex justify-center" on:click={() => dropdown=!dropdown}>Profiles 
			<Icon src={dropdown ? ChevronUp : ChevronDown} solid class="w-6 h-6 mt-1 mx-2"/>
		</button></div>
		
		<div class:hidden={!dropdown} class="sm:block">
			{#each tabs as tab, i (i)}
				<button class="tab flex" class:tab-current={current_tab === tab} on:click={() => {current_tab = tab}}>
					<span class="w-full text-start">{tab}</span>
					{#if current_tab === tab}
						<button class="btn-secondary justify-self-end w-12" on:click={()=>{}}><Icon src={PencilSquare} class="w-5 h-5 mt-1 mx-2"/></button>
						<button class="btn-secondary justify-self-end w-12" on:click={()=>{}}><Icon src={Trash} class="w-5 h-5 mt-1 mx-2"/></button>
					{/if}
				</button>
			{/each}
			<button class="tab border border-indigo-200" on:click={()=>{}}>+</button>
		</div>
		
	</div>
	
	<div class="w-full mt-8 z-0">
		<Calculator bind:table_name={current_tab}/>
	</div>
		
</div>

<style>
.tab {
	@apply bg-indigo-300 text-white font-bold text-lg p-2 px-4 rounded-xl text-center w-full transition duration-200 ease-in-out;
}
.tab-current {
	@apply bg-indigo-400 text-white font-bold text-lg p-2 px-4 rounded-xl text-center w-full;
}

.tab:hover{
	@apply bg-indigo-400;
}

.tab:active{
	@apply bg-indigo-500;
}

.btn-secondary {
	@apply text-gray-300 text-lg rounded-xl text-center transition duration-200 ease-in-out;
}
.btn-secondary:hover {
	@apply text-white;
}


</style>