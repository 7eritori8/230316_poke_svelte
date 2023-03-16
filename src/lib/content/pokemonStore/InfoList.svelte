<script>
	import { pokemonStore } from '../../module/pokemonStore/pokemonStore';
	function onDelete(i) {
		pokemonStore.update((n) => {
			n.splice(i, 1);
			return n;
		});
	}
</script>

<div class="mt-6 flow-root">
	{#if $pokemonStore.length !== 0}
		<ul role="list" class="-my-5 divide-y divide-gray-200 mb-4">
			{#each $pokemonStore as poke, i}
				<li class="py-1">
					<div class="flex items-center space-x-4">
						<div class="flex-shrink-0">
							<img class="h-12 w-12 rounded-full" src={poke.pokeImg} alt="" />
						</div>
						<div class="min-w-0 flex-1">
							<p class="truncate text-sm font-medium text-gray-900">{poke.pokeName}</p>
						</div>
						<div>
							<button
								on:click={(i) => {
									onDelete();
								}}
								class="inline-flex items-center rounded-full bg-white px-2.5 py-1 text-xs font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
							>
								にがす
							</button>
						</div>
					</div>
				</li>
			{/each}
		</ul>
	{:else}
		<h1>まだポケモンをつかまえていないよ！</h1>
	{/if}
</div>
<a href="/edit">ポケモンをさがしにいく！</a>

<style>
	a {
		font-size: 16px;
	}
</style>
