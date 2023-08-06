<script lang="ts">
	let textareaValue = ``;
	let names: string[] = [];
	$: names = textareaValue.split('\n').filter((name) => name.trim() !== '');
	let groupSize = 3;

	let groups: string[][] = [];
	$: groups = Number(groupSize)
		? Array.from({ length: Math.ceil(names.length / groupSize) }).map((_, i) =>
				names.slice(i * groupSize, (i + 1) * groupSize)
		  )
		: [];

	function randomize() {
		console.log('randomize');
		names = names.sort(() => 0.5 - Math.random());
	}
</script>

<h1>Grouper</h1>

<label>
	Enter student names seperated by line:
	<textarea bind:value={textareaValue} />
</label>

<label>
	Group size:
	<input type="number" min="2" max="100" bind:value={groupSize} />
</label>

<button on:click={randomize}> Randomize </button>

{#if names.length > 0 && Number(groupSize)}
	<div style="display: flex; flex-wrap: wrap">
		{#each Array.from({ length: Math.ceil(names.length / groupSize) }) as _, i}
			<div class="group">
				<b>Group {i + 1}</b>
				{#each names.slice(i * groupSize, (i + 1) * groupSize) as name}
					<div>
						{name}
					</div>
				{/each}
			</div>
		{/each}
	</div>
{/if}

<style>
	.group {
		margin: 1rem;
	}
</style>
