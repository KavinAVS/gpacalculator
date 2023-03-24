<script>
	export let table_name; 

	let aim = 80;
	let count = 7;
	let marks = Array(count);
	let weights = Array(count);
	let names = Array(count);
	
	
	$: getTableVal(table_name);
	function getTableVal(table_name){
		if(table_name in localStorage){
			let vals = JSON.parse(localStorage.getItem(table_name));
			count = vals.marks.length < 7 ? 7 : vals.marks.length;
			marks = vals.marks;
			weights = vals.weights;
			names = vals.names;
		}else{
			count = 7;
			marks = Array(count);
			weights = Array(count);
			names = Array(count);
		}
	}

	function removeEntry(id){
		marks = [...marks.slice(0, id), ...marks.slice(id + 1)];
		weights = [...weights.slice(0, id), ...weights.slice(id + 1)];
		names = [...names.slice(0, id), ...names.slice(id + 1)];
		count = count - 1;
	}

	$:total_weight = weights.reduce((sum, n) => sum+n,0);
	$:completed_average = marks.reduce((sum, n, i) =>{ return (sum + (n*weights[i])) }, 0)
	$:current_average =  completed_average/total_weight;
	$:needed = (aim - (completed_average/100))/ (100-total_weight)*100;

	
	$: saveToLocalStrg(names,marks,weights);
	function saveToLocalStrg(names, marks, weights){
		localStorage.setItem(table_name, JSON.stringify({"names": names, "marks": marks, "weights": weights}));
	}

</script>

<svelte:head>
	<title>Grade Calculator</title>
	<meta name="grade-calculator" content="" />
</svelte:head>

<div class="min-w-full flex justify-center items-center flex-col">
	<div class="max-w-xl columns-1 w-11/12">
		<div class="gap-2 md:gap-4 flex mb-2">
			<h5 class="w-1/2">Name</h5>
			<h5 class="w-1/4">Mark</h5>
			<h5 class="w-1/4">Weight</h5>
			<p class="w-12"></p>
		</div>
		{#each Array(count) as _, i (i)}
			<div class="gap-2 md:gap-4 min-w-full flex justify-evenly mb-2">
				<input name={`name${i}`} bind:value={names[i]} type="text" class="w-1/2 border-solid border-2 border-neutral-500 rounded-md p-2">
				<input name={`mark${i}`} bind:value={marks[i]} min=0 max=100 type="number" placeholder="%" class="w-1/4 border-solid border-2 border-neutral-500 rounded-md p-2 text-right">
				<input name={`weight${i}`} bind:value={weights[i]} min=0 max=100 type="number" class="w-1/4 border-solid border-2 border-neutral-500 rounded-md p-2 text-right">
				<button class="btn" on:click={removeEntry(i)}>-</button>
			</div>
		{/each}
		<button class="btn w-full" on:click={() => count = count + 1 }>+</button>
	</div>
	<p>Average: {current_average.toFixed(2)}%</p>
	<span>What do I need to get: <input name="aim" bind:value={aim} min=0 max=100 type="number" class="border-solid border-2 border-neutral-500 rounded-md px-2 py-1 text-right">%</span>
	<p>Whats needed: {needed.toFixed(2)}%</p>
</div>


<style>
input::-webkit-outer-spin-button,input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

.btn {
	@apply bg-indigo-300 text-white font-bold rounded-md p-2 px-4 text-center transition duration-200 ease-in-out;
}
.btn:hover{
	@apply bg-indigo-400;
}

.btn:active{
	@apply bg-indigo-500;
}

</style>
