<script>
	import { Icon, ChevronDown, ChevronUp, PencilSquare, Trash, XMark, Check } from "svelte-hero-icons";
	import Calculator from "./Calculator.svelte";

	
	let tabs = Object.keys(localStorage);
	if(tabs.length === 0)tabs = ["New Course"];
	tabs.sort();
	
	let current_tab = tabs[0];
	let dropdown = false;

	let edit = false;
	let edit_bind = "";

	function makeNewTab(name){
		tabs = [...tabs, get_unique_name(name)];
	}

	function deleteTab(name){
		localStorage.removeItem(name);
		let i = tabs.indexOf(name);
		if(i !== -1) tabs = [...tabs.slice(0, i), ...tabs.slice(i + 1)];
		if(tabs.length === 0)tabs = ["New Course"];
	}

	function renameTab(name, edit_bind){
		if(name === edit_bind) return;

		let n_name = get_unique_name(edit_bind);

		localStorage.setItem(n_name, localStorage.getItem(name));
		localStorage.removeItem(name);

		let i = tabs.indexOf(name);
		if(i !== -1) tabs = [...tabs.slice(0, i), n_name, ...tabs.slice(i + 1)];
	}

	function get_unique_name(name){
		if(name === undefined) name = "New Course";
		let n_name = name;
		let n = 0;
		while(tabs.includes(n_name)){
			n_name = name+n;
			n = n+1;
		}
		return n_name;
	}

</script>

<div class="w-full sm:flex gap-8 h-screen">
	
	<div class="w-full sm:block sm:w-48 md:w-80 columns-1 shadow bg-indigo-300 p-2 flex-shrink-0">

		<h1 class="hidden sm:block text-white text-center pb-8 pt-5 text-2xl font-sans font-bold underline underline-offset-8">Grade Calculator</h1>
		<div><button class="sm:hidden tab flex justify-center" on:click={() => dropdown=!dropdown}>Profiles 
			<Icon src={dropdown ? ChevronUp : ChevronDown} solid class="w-6 h-6 mt-1 mx-2"/>
		</button></div>
		
		<div class:hidden={!dropdown} class="sm:block">
			{#each tabs as tab, i (i)}
				{#if (current_tab===tab && edit === true)}
					<div class="tab flex" class:tab-current={current_tab === tab} >
						<input type="text" required class="bg-indigo-300 w-full rounded-md text-start whitespace-nowrap text-ellipsis" bind:value={edit_bind} />
						<button class="btn-secondary justify-self-end w-12" on:click={()=>{renameTab(tabs[i], edit_bind); edit=false;}}><Icon src={Check} class="w-5 h-5 mt-1 mx-2"/></button>
						<button class="btn-secondary justify-self-end w-12" on:click={()=>{edit=false}}><Icon src={XMark} class="w-5 h-5 mt-1 mx-2"/></button>
					</div>
				{:else}
					<button class="tab flex" class:tab-current={current_tab === tab} on:click={() => {current_tab = tab; edit=false}}>
						<p class="w-full text-start overflow-x-hidden whitespace-nowrap text-ellipsis">{tab}</p>
						{#if current_tab === tab}
							<button class="btn-secondary justify-self-end w-12" on:click={()=>{edit=true; edit_bind=tab}}><Icon src={PencilSquare} class="w-5 h-5 mt-1 mx-2"/></button>
							<button class="btn-secondary justify-self-end w-12" on:click={()=>{deleteTab(tab)}}><Icon src={Trash} class="w-5 h-5 mt-1 mx-2"/></button>
						{/if}
					</button>
				{/if}
			{/each}
			<button class="tab border border-indigo-200" on:click={()=>{makeNewTab()}}>+</button>
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