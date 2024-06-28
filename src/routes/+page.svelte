<script lang="ts">
	export let data;

	let index = 0;
	let count = data.dhikr[index].count;

	function decreaseCount() {
		if (count - 1 < 1) {
			nextDhikr();
		} else {
			count--;
		}
	}

	function nextDhikr() {
		if (index !== data.dhikr.length - 1) {
			index = index + 1;
			count = data.dhikr[index + 1].count;
		}
	}

	function prevDhikr() {
		if (index > 0) {
			index = index - 1;
			count = data.dhikr[index - 1].count;
		}
	}

	$: {
		count = data.dhikr[index].count;
	}
</script>

<svelte:document
	on:keydown={(e) => {
		if (e.key === ' ' || e.key === 'Space') {
			decreaseCount();
		}
	}}
/>

<div class="relative h-screen w-full grid grid-cols-3 items-center p-6">
	<ul class="w-64 max-h-screen overflow-auto">
		{#each data.dhikr as dhikr, i (i)}
			<li>
				<button
					class={`hover:text-black/60 ${index === i ? 'underline' : ''}`}
					on:click={() => (index = i)}
				>
					{i + 1}. {dhikr.dhikr.length > 20 ? `${dhikr.dhikr.substring(0, 20)}...` : dhikr.dhikr}
				</button>
			</li>
		{/each}
	</ul>
	<div class="h-full flex flex-col items-center justify-between">
		<div class="h-1/3"></div>
		<button
			type="button"
			class="max-w-96 w-full h-1/3 aspect-square text-center hover:text-black/50 overflow-auto"
			on:click={decreaseCount}
		>
			{data.dhikr[index].quran ?? data.dhikr[index].dhikr}
		</button>
		<div class="h-1/3 flex flex-col items-center justify-center">
			<h2 class="text-center">عدد المرات: {count}</h2>
			<div class="inline-flex w-full items-center justify-center gap-6">
				<button
					type="button"
					class="px-4 py-2 flex-grow-0 hover:bg-black/10 disabled:hover:bg-inherit disabled:opacity-50 rounded-md"
					on:click={prevDhikr}
					disabled={index === 0}>قبل</button
				>
				<button
					type="button"
					class="px-4 py-2 flex-grow-0 hover:bg-black/10 disabled:hover:bg-inherit disabled:opacity-50 rounded-md"
					on:click={nextDhikr}
					disabled={index === data.dhikr.length - 1}>بعد</button
				>
			</div>
			<button
				class="text-sm text-black/80"
				on:click={() => {
					document.getElementById('help-popover')?.classList.toggle('hidden');
				}}
			>
				معلومات
			</button>
			<div
				id="help-popover"
				class="absolute left-0 bottom-0 border hidden m-2 p-2 border-black rounded-md backdrop:blur-lg"
			>
				<p>الضغط على الذكر في وصط الشاشه أو على مفتاح "Space" ينقص عدد مرات الذكر</p>
			</div>
		</div>
	</div>
</div>
